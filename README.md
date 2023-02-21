# Value-at-Risk-VAR-of-TCS
The calculation of VAR of TCS using Python
# Value at Risk Assessment Model
# Import packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import yfinance as yf

### Distributions - Set Up Stock Parameters

# Number of shares
shares_TCS = 1000

# Live stock price
price = yf.Ticker('TCS.NS')
price_TCS = price.history().tail(1)['Close'].iloc[0]

print(price)
print('---------')
print(price_TCS)

# Investment value
value_TCS = price_TCS * shares_TCS

# Risk free rate (10 year Treasury Rate)
rfr_TCS = 0.035

# Volatility (30-day volatility at that time)
vol_TCS = 0.0127
print(value_TCS)

### Simulations - Calculate Investment Returns

# Number of simulations
simulations = 5000 

# Investment time in a year
t_TCS = 21/252 

# Explain np.random.standard_normal
sample = np.random.standard_normal(5000)
plt.hist(sample,bins=100)

print(np.mean(sample))
print(np.std(sample))

# Create a function to calculate the returns of the investment
def VaR(pv, rfr, vol, t, simulations):
    end_value = pv * np.exp((rfr - .5 * vol ** 2) * t + np.random.standard_normal(
        simulations) * vol * np.sqrt(t))
    returns = end_value - pv
    return returns

# Apply the VaR function to simulate the returns
returns_TCS = VaR(value_TCS, rfr_TCS, vol_TCS, t_TCS, simulations)
print(returns_TCS)

### Quantifications - Identify VaR at 90%, 95%, and 99% Confidence Level

# Explain string formatting
# Print: VaR at x% confidence level: $y.

x = 0.9
y = price_TCS
print("VaR at {:.0%} confidence level:  ₹{:,.0f}".format(x, y))

# Plot the returns
plt.hist(returns_TCS, bins=100);

# Show VaR at 90%, 95%, and 99% confidence level
percentiles = [10,5,1]

for i in percentiles:
    confidence = (100-i)/100
    value = np.percentile(returns_TCS, i)
    print("VaR at {:.0%} confidence level:  ₹{:,.0f}".format(confidence, value))
    plt.axvline(value, color = 'red', linestyle='dashed', linewidth=1)


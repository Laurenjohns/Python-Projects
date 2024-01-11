# Python-Projects

---
## Table of Contents:

- [S&P 500 Web Scraping](#sp-500-web-scraping)
- [Stock Analysis](#stock-analysis)
- [Call Center Simulation](#call-center-simulation)
  
---

### S&P 500 Web Scraping

Project Overview: Utilizing Python for web scraping. 

```python

# web scrap and load

import bs4 as bs
import requests
# web scrap and save using import pickle
import pickle

html = requests.get('https://en.wikipedia.org/wiki/List_of_S%26P_500_companies')
soup = bs.BeautifulSoup(html.text)

tickers = []

table = soup.find('table', {'class': 'wikitable sortable'})
rows = table.findAll('tr')[1:]
for row in rows:
  ticker = row.findAll('td')[0].text
  tickers.append(ticker[:-1])
# load

print(tickers)

# saves
with open('snp500.pickle', 'wb') as f:
    pickle.dump(tickers, f)

# below is used after web scraping and saving to reload data again.

import pickle
with open('snp500.pickle', 'rb') as f:
     tickers = pickle.load(f)
  
print(tickers)
```

---

### Stock Analysis  

Project Overview: Leveraging Python to create a stock analysis of Apple's RSI.

```python
import pandas as pd
import yfinance as yf
import matplotlib.pyplot as plt 
import datetime as dt

ticker = 'AAPL'
start = dt.datetime(2018, 1, 1)
end = dt.datetime.now()

data = web.DataReader(ticker, 'yahoo', start, end)

delta = data['Adj Close'].diff(1)
delta.dropna(inplace=True)

positive = delta.copy()
negative = delta.copy()

positive[positive < 0] = 0
negative[negative > 0] = 0 

days = 14

average_gain = positive.rolling(window=days).mean()
average_loss = abs(negative.rolling(window=days).mean())

relative_strength = average_gain / average_loss 
RSI = 100.0 - (100.0 / (1.0 + relative_strength))

combined = pd.DataFrame()
combined['Adj Close'] = data['Adj Close'] 
combined['RSI'] = RSI

plt.figure(figsize=(12,8))
ax1 = plt.subplot(211)
ax1.plot(combined.index, combined['Adj Close'], color='lightgray')
ax1.set_title("Adjusted Close Price", color='white')

ax1.grid(True, color='#555555')
ax1.set_axisbelow(True)
ax1.set_facecolor('black')
ax1.figure.set_facecolor('black')
ax1.tick_params(axis='x', colors='white')
ax1.tick_params(axis='y', colors='white')

ax2 = plt.subplot(212, sharex=ax1)
ax2.plot(combined.index, combined['RSI'], color='lightgray')
ax2.axhline(0, linestyle='--', alpha=0.5, color='#ff0000')
ax2.axhline(10, linestyle='--', alpha=0.5, color='#ffaa00')
ax2.axhline(20, linestyle='--', alpha=0.5, color='#00ff00')
ax2.axhline(30, linestyle='--', alpha=0.5, color='#cccccc')
ax2.axhline(70, linestyle='--', alpha=0.5, color='#cccccc')
ax2.axhline(80, linestyle='--', alpha=0.5, color='#00ff00')
ax2.axhline(90, linestyle='--', alpha=0.5, color='#ffaa00')
ax2.axhline(100, linestyle='--', alpha=0.5, color='#ff0000')

ax2.set_title('RSI Value')
ax2.grid(False)
ax2.set_axisbelow(True)
ax2.set_facecolor('black')
ax2.tick_parmas(axis='x', colors='white')
ax2.tick_params(axis='y', colors='white')

plt.show()
```
---

### Call Center Simulation 

Project Overview:  Real-Time performance monitoring of a call center using Python, Microsoft SQL Server, and Power BI. The simulation will generate random calls from customers and store the call center information in a Microsoft SQL server database for performance monitoring. Utilizing the insert command and connecting Python to the SQL server for automatic data entry.

```python
import datetime
import time
import random

while 1==1:
    date = datetime.date.today()
    location = random.randint(111, 201)
    company = random.randint(11,30)
    issue = random.randint(111,130)
    csr = random.randint(111,210)
    rtime = random.randint(1,300)
    ctime = random.randint(1,600)
    status = random.randint(1,4)
    if status == 1:
        rating = random.randint(5,10)
    elif status == 2:
        rating = random.randint(3,8)
    elif status == 3:
        rating = random.randint(1,3)
    elif status == 4:
        rating = random.randint(1,5)
        
    print(date)
    print(location)
    print(company)
    print(issue)
    print(csr)
    print(rtime)
    print(ctime)
    print(status)
    print(rating)    
    time.sleep(1)
```
---

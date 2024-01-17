# Python-Projects

---
## Table of Contents:

- [Kaggle API](#kaggle-api)
- [S&P 500 Web Scraping](#sp-500-web-scraping)
- [Stock Analysis](#stock-analysis)
- [Call Center Simulation](#call-center-simulation)
  
---

### Kaggle API

Project Overview: Collected data programatically using the Kaggle API, utilizing data exploration, assessment and manipulation with the pandas library in Python.

```python
import pandas as pd
import zipfile
import kaggle

!kaggle datasets download -d hmavrodiev/london-bike-sharing-dataset

zipfile_name = 'london-bike-sharing-dataset.zip'
with zipfile.ZipFile(zipfile_name, 'r') as file:
  file.extractall()

bikes = pd.read_csv("london_merged.csv")
bikes.info()

bikes.shape
bikes
bikes.weather_code.value_counts()
bikes.season.value_counts()

new_cols_dict ={
    'timestamp' : 'time', 
    'cnt' : 'count',
     't1': 'temp_real_c',
    't2': 'temp_feels_like_c',
    'hum': 'humidity_percent',
    'wind_speed': 'wind_speed_kph',
    'weather_code': 'weather',
    'is_holiday' : 'is_holiday', 
    'is_weekend' : 'is_weekend',
    'season' : 'season'
}

bikes.rename(new_cols_dict, axis=1, inplace=True)

bikes.humidity_percent = bikes.humidity_percent / 100

season_dict = {
    '0.0':'spring',
    '1.0':'summer',
    '2.0':'autumn',
    '3.0':'winter'
}

weather_dict = {
     '1.0': 'Clear',
    '2.0': 'Scattered clouds',
    '3.0': 'Broken clouds',
    '4.0': 'Cloudy',
    '7.0': 'Rain',
    '10.0':'Rain with thunderstorm',
    '26.0':'Snowfall'
}

bikes.season = bikes.season.astype('str')
bikes.season = bikes.season.map(season_dict)

bikes.weather = bikes.weather.astype('str')
bikes.weather = bikes.weather.map(weather_dict)

bikes.head()

# saves new data file to excel
bikes.to_excel('london_bikes_final.xlsx', sheet_name='Data')
```
<img width="656" alt="Screenshot 2024-01-16 142904" src="https://github.com/Laurenjohns/Python-Projects/assets/107310914/067bda9a-333a-478a-8e76-34454a728b35">

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

<img width="506" alt="Screenshot 2024-01-11 170228" src="https://github.com/Laurenjohns/Python-Projects/assets/107310914/798c7715-d3e7-42f4-a0f6-b17df229195d">

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

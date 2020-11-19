# DJI--Trading-Strategies-

# Dow Jones Industrial Average Trading Strategies

## Getting Started.

1. Clone this repo [Github Pages](https://github.com/Dantarshi/DJI--Trading-Strategies-).

2. Raw Data is gotten from ("https://en.wikipedia.org/wiki/Dow_Jones_Industrial_Average").

3. main notebook link [Github Notebook](https://github.com/Dantarshi/DJI--Trading-Strategies-/blob/main/DJI%20Strategy.ipynb)


## Project Intro/Objective
The purpose of this project is to use stock historical data to develop the best strategy for trading Dow Jones average and to determine the risk associated with the specific strategy.


## Methods Used.
* Web Scrapping.
* Data Visualization.
* etc.

## Technologies 
* Python.
* Seaborn.
* Matplotlib
* Pandas, jupyter.
* Unicodedata
* etc. 

## Project Description.
Dow Jones Industrial Average measures the stock performance of 30 large companies listed on the stock exchanges in the United State.It is the most commonly followed indices in U.S stock market. 
In this project, we will attempt to use the Dow Jones to develop a strategy that will yield profitability over time. although Dow Jones is not traded directly, however, we can invest in a portfolio that has constituents of Dow Jones.

We might know little or nothing about Dow Jones however, we can make use of wikipedia to help us in the process of getting the vital components to use in this project.

We will do a little web scrapping to get the ticker symbols to pull the data from the web API needed for the project.

## Needs of this project.

- data exploration/descriptive statistics.
- data processing/cleaning.
- visualization.
- writeup/reporting.

Research Question: What is the best trading strategy fir Dow Jones and the risk associated with it? 

## Importing and cleaning data.

Data for this project is gotten from https://en.wikipedia.org/wiki/Dow_Jones_Industrial_Average and yahoo finance.


## Data cleaning.

The cleaning of the data includes:
* Checking for null or missing data.
* Getting information on data types.
* Checking the data shapes.
* Getting data summary.
* Droping the null values(since there was only one row missing).
* Checking for duplicates.
* Checking for place holders.








The 30 constituente of Dow Jones

    0      MMM
    1      AXP
    2     AMGN
    3     AAPL
    4       BA
    5      CAT
    6      CVX
    7     CSCO
    8       KO
    9      DOW
    10      GS
    11      HD
    12     HON
    13     IBM
    14    INTC
    15     JNJ
    16     JPM
    17     MCD
    18     MRK
    19    MSFT
    20     NKE
    21      PG
    22     CRM
    23     TRV
    24     UNH
    25      VZ
    26       V
    27     WBA
    28     WMT
    29     DIS
  













#### Dow Jones chart 


![png](output_41_0.png)



#### A plot of Dow Jones Chart and Return on the secondary axis


![png](output_44_0.png)




Positions:

+1: Investing in DJI (Long position)

-1: Short Selling DJI (Short Position)

0: No Position (Neutral)

Strategies:

-Buy and Hold ( Basic strategy - passive): Initial investing in DJI and do nothing(Position: +1 on any given day)

Simple Momentum (active strategy to be tested):

1.) Investing (+1) into DJI tomorrow id today return was positive

2.) short selling(-1) tomorrow if today return was negative


#### The plotting our strategy against buy and hold strategy



![png](output_51_0.png)



```python
# We have 252 trading days so we will make a function to calculate return and risk

def summary_ann(returns):
    summary = returns.agg(["mean", "std"]).T
    summary["Return"] = summary["mean"]*252
    summary["Risk"] = summary["std"]* np.sqrt(252)
    summary.drop(columns = ["mean", "std"], inplace = True)
    return summary
```



### Back Testing Simple Constrarian Strategy

Strategies

-Simple Constrarian (test)
1.) Short Sell (-1) DJI tomorrow if today's return was positive.

2.) Invest (+1) into DJI tomorrow if today's return was negative.


![png](output_62_0.png)



#### Forward Testing

##### Fitting

When we plot our strategy and compare it with DJI

```python
# Plotting dji and our strategy
df_f[["DJI_Close", "Strategy"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.title("Simple Contrarian Strategy", fontsize = 20)
plt.show()
```


![png](output_72_0.png)


Our strategy out performed DJI buy and hold strategy.


We found a strategy that out performed our basic buy and hold strategy. We still need to further anlysis. . Backtesting vs Fitting was fitted and optimized on historic data. forward testing is definately required. Positional chages require cost in some trading platforms and that should be taken into consideration. Tax effects should be taken nto consideration as well

DJI Return = 10.48%, Risk = 17.41%
Fitting Strategy =  20.90%, Risk = 17.37%


#### Simple Moving Averages


```python
# converting to dataframe for easier manipulation
df_s = dji["Close"].to_frame()
```


```python
# creating dji close and return columns
df_s["DJI_Return"] = df_s.Close.pct_change()
df_s.columns = ["DJI_Close", "DJI_Return"]
df_s.dropna(inplace = True)
df_s
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DJI_Close</th>
      <th>DJI_Return</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2010-01-05</td>
      <td>10572.019531</td>
      <td>-0.001128</td>
    </tr>
    <tr>
      <td>2010-01-06</td>
      <td>10573.679688</td>
      <td>0.000157</td>
    </tr>
    <tr>
      <td>2010-01-07</td>
      <td>10606.860352</td>
      <td>0.003138</td>
    </tr>
    <tr>
      <td>2010-01-08</td>
      <td>10618.190430</td>
      <td>0.001068</td>
    </tr>
    <tr>
      <td>2010-01-11</td>
      <td>10663.990234</td>
      <td>0.004313</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2020-09-28</td>
      <td>27584.060547</td>
      <td>0.015092</td>
    </tr>
    <tr>
      <td>2020-09-29</td>
      <td>27452.660156</td>
      <td>-0.004764</td>
    </tr>
    <tr>
      <td>2020-09-30</td>
      <td>27781.699219</td>
      <td>0.011986</td>
    </tr>
    <tr>
      <td>2020-10-01</td>
      <td>27816.900391</td>
      <td>0.001267</td>
    </tr>
    <tr>
      <td>2020-10-02</td>
      <td>27682.810547</td>
      <td>-0.004820</td>
    </tr>
  </tbody>
</table>
<p>2706 rows × 2 columns</p>
</div>




```python
# creating a 20 day moving average
df_s.DJI_Close.rolling(window = 20).mean()
```




    Date
    2010-01-05             NaN
    2010-01-06             NaN
    2010-01-07             NaN
    2010-01-08             NaN
    2010-01-11             NaN
                      ...     
    2020-09-28    27779.850781
    2020-09-29    27730.981250
    2020-09-30    27687.783203
    2020-10-01    27623.603223
    2020-10-02    27593.107227
    Name: DJI_Close, Length: 2706, dtype: float64




```python
# adding the sma20 to our frame
df_s["SMA20"] = df_s.DJI_Close.rolling(window = 20).mean()
```


```python
# plotting the dji and sma20
df_s[["DJI_Close", "SMA20"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.show()
```


![png](output_82_0.png)



```python
# creating a 50 day moving average
df_s.DJI_Close.rolling(window = 50).mean()
```




    Date
    2010-01-05             NaN
    2010-01-06             NaN
    2010-01-07             NaN
    2010-01-08             NaN
    2010-01-11             NaN
                      ...     
    2020-09-28    27546.211953
    2020-09-29    27561.647773
    2020-09-30    27580.473750
    2020-10-01    27596.694961
    2020-10-02    27617.304570
    Name: DJI_Close, Length: 2706, dtype: float64




```python
# adding the sma20 to our frame
df_s["SMA50"] = df_s.DJI_Close.rolling(window = 50).mean()
```


```python
# plotting the dji and sma50
df_s[["DJI_Close", "SMA50"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.show()
```


![png](output_85_0.png)



```python
# creating a 200 day moving average
df_s.DJI_Close.rolling(window = 200).mean()
```




    Date
    2010-01-05             NaN
    2010-01-06             NaN
    2010-01-07             NaN
    2010-01-08             NaN
    2010-01-11             NaN
                      ...     
    2020-09-28    26277.101426
    2020-09-29    26273.704473
    2020-09-30    26271.936064
    2020-10-01    26269.841113
    2020-10-02    26266.919365
    Name: DJI_Close, Length: 2706, dtype: float64




```python
# adding the sma200 to our frame
df_s["SMA200"] = df_s.DJI_Close.rolling(window = 200).mean()
```


```python
# plotting dji and sma200
df_s[["DJI_Close", "SMA200"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.show()
```


![png](output_88_0.png)



```python
# plot both sma20, sma50 and sma 200 on same graph
df_s[["DJI_Close","SMA20","SMA50", "SMA200"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.show()
```


![png](output_89_0.png)


 #### Crossovers Strategy


```python
# droppign th NanN values
df_s.dropna(inplace =True)
df_s
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DJI_Close</th>
      <th>DJI_Return</th>
      <th>SMA20</th>
      <th>SMA50</th>
      <th>SMA200</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2010-10-19</td>
      <td>10978.620117</td>
      <td>-0.014813</td>
      <td>10920.518018</td>
      <td>10582.089805</td>
      <td>10507.098203</td>
    </tr>
    <tr>
      <td>2010-10-20</td>
      <td>11107.969727</td>
      <td>0.011782</td>
      <td>10938.951025</td>
      <td>10591.364199</td>
      <td>10509.777954</td>
    </tr>
    <tr>
      <td>2010-10-21</td>
      <td>11146.570312</td>
      <td>0.003475</td>
      <td>10963.158545</td>
      <td>10606.719004</td>
      <td>10512.642407</td>
    </tr>
    <tr>
      <td>2010-10-22</td>
      <td>11132.559570</td>
      <td>-0.001257</td>
      <td>10976.773535</td>
      <td>10622.971191</td>
      <td>10515.270903</td>
    </tr>
    <tr>
      <td>2010-10-25</td>
      <td>11164.049805</td>
      <td>0.002829</td>
      <td>10994.374023</td>
      <td>10640.189180</td>
      <td>10518.000200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2020-09-28</td>
      <td>27584.060547</td>
      <td>0.015092</td>
      <td>27779.850781</td>
      <td>27546.211953</td>
      <td>26277.101426</td>
    </tr>
    <tr>
      <td>2020-09-29</td>
      <td>27452.660156</td>
      <td>-0.004764</td>
      <td>27730.981250</td>
      <td>27561.647773</td>
      <td>26273.704473</td>
    </tr>
    <tr>
      <td>2020-09-30</td>
      <td>27781.699219</td>
      <td>0.011986</td>
      <td>27687.783203</td>
      <td>27580.473750</td>
      <td>26271.936064</td>
    </tr>
    <tr>
      <td>2020-10-01</td>
      <td>27816.900391</td>
      <td>0.001267</td>
      <td>27623.603223</td>
      <td>27596.694961</td>
      <td>26269.841113</td>
    </tr>
    <tr>
      <td>2020-10-02</td>
      <td>27682.810547</td>
      <td>-0.004820</td>
      <td>27593.107227</td>
      <td>27617.304570</td>
      <td>26266.919365</td>
    </tr>
  </tbody>
</table>
<p>2507 rows × 5 columns</p>
</div>



Strategies:
    
- Buy and Hold (Basic Strategy): buy and do nothing.
    
- SMA Crossover (Momentum)(Test Strategy):
    1.) Invest (+1): SMA50 > SMA200
    2.) Short Sell (-1): SMA50 < SMA200


```python
# adding our position column
df_s["Position"] = np.sign(df_s.SMA50.sub(df_s.SMA200))
df_s
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DJI_Close</th>
      <th>DJI_Return</th>
      <th>SMA20</th>
      <th>SMA50</th>
      <th>SMA200</th>
      <th>Position</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2010-10-19</td>
      <td>10978.620117</td>
      <td>-0.014813</td>
      <td>10920.518018</td>
      <td>10582.089805</td>
      <td>10507.098203</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2010-10-20</td>
      <td>11107.969727</td>
      <td>0.011782</td>
      <td>10938.951025</td>
      <td>10591.364199</td>
      <td>10509.777954</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2010-10-21</td>
      <td>11146.570312</td>
      <td>0.003475</td>
      <td>10963.158545</td>
      <td>10606.719004</td>
      <td>10512.642407</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2010-10-22</td>
      <td>11132.559570</td>
      <td>-0.001257</td>
      <td>10976.773535</td>
      <td>10622.971191</td>
      <td>10515.270903</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2010-10-25</td>
      <td>11164.049805</td>
      <td>0.002829</td>
      <td>10994.374023</td>
      <td>10640.189180</td>
      <td>10518.000200</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2020-09-28</td>
      <td>27584.060547</td>
      <td>0.015092</td>
      <td>27779.850781</td>
      <td>27546.211953</td>
      <td>26277.101426</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2020-09-29</td>
      <td>27452.660156</td>
      <td>-0.004764</td>
      <td>27730.981250</td>
      <td>27561.647773</td>
      <td>26273.704473</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2020-09-30</td>
      <td>27781.699219</td>
      <td>0.011986</td>
      <td>27687.783203</td>
      <td>27580.473750</td>
      <td>26271.936064</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2020-10-01</td>
      <td>27816.900391</td>
      <td>0.001267</td>
      <td>27623.603223</td>
      <td>27596.694961</td>
      <td>26269.841113</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2020-10-02</td>
      <td>27682.810547</td>
      <td>-0.004820</td>
      <td>27593.107227</td>
      <td>27617.304570</td>
      <td>26266.919365</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>2507 rows × 6 columns</p>
</div>




```python
# Plotting our position as a secondary axis
df_s[["SMA20","SMA50", "SMA200", "Position"]].plot(figsize = (15,10), secondary_y = "Position", fontsize = 15)
plt.show()
```


![png](output_94_0.png)



```python
# calculation the return on our strategy.
df_s["Strategy_Ret"] = df_s["Position"].shift() * df_s["DJI_Return"]
df_s
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DJI_Close</th>
      <th>DJI_Return</th>
      <th>SMA20</th>
      <th>SMA50</th>
      <th>SMA200</th>
      <th>Position</th>
      <th>Strategy_Ret</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2010-10-19</td>
      <td>10978.620117</td>
      <td>-0.014813</td>
      <td>10920.518018</td>
      <td>10582.089805</td>
      <td>10507.098203</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2010-10-20</td>
      <td>11107.969727</td>
      <td>0.011782</td>
      <td>10938.951025</td>
      <td>10591.364199</td>
      <td>10509.777954</td>
      <td>1.0</td>
      <td>0.011782</td>
    </tr>
    <tr>
      <td>2010-10-21</td>
      <td>11146.570312</td>
      <td>0.003475</td>
      <td>10963.158545</td>
      <td>10606.719004</td>
      <td>10512.642407</td>
      <td>1.0</td>
      <td>0.003475</td>
    </tr>
    <tr>
      <td>2010-10-22</td>
      <td>11132.559570</td>
      <td>-0.001257</td>
      <td>10976.773535</td>
      <td>10622.971191</td>
      <td>10515.270903</td>
      <td>1.0</td>
      <td>-0.001257</td>
    </tr>
    <tr>
      <td>2010-10-25</td>
      <td>11164.049805</td>
      <td>0.002829</td>
      <td>10994.374023</td>
      <td>10640.189180</td>
      <td>10518.000200</td>
      <td>1.0</td>
      <td>0.002829</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2020-09-28</td>
      <td>27584.060547</td>
      <td>0.015092</td>
      <td>27779.850781</td>
      <td>27546.211953</td>
      <td>26277.101426</td>
      <td>1.0</td>
      <td>0.015092</td>
    </tr>
    <tr>
      <td>2020-09-29</td>
      <td>27452.660156</td>
      <td>-0.004764</td>
      <td>27730.981250</td>
      <td>27561.647773</td>
      <td>26273.704473</td>
      <td>1.0</td>
      <td>-0.004764</td>
    </tr>
    <tr>
      <td>2020-09-30</td>
      <td>27781.699219</td>
      <td>0.011986</td>
      <td>27687.783203</td>
      <td>27580.473750</td>
      <td>26271.936064</td>
      <td>1.0</td>
      <td>0.011986</td>
    </tr>
    <tr>
      <td>2020-10-01</td>
      <td>27816.900391</td>
      <td>0.001267</td>
      <td>27623.603223</td>
      <td>27596.694961</td>
      <td>26269.841113</td>
      <td>1.0</td>
      <td>0.001267</td>
    </tr>
    <tr>
      <td>2020-10-02</td>
      <td>27682.810547</td>
      <td>-0.004820</td>
      <td>27593.107227</td>
      <td>27617.304570</td>
      <td>26266.919365</td>
      <td>1.0</td>
      <td>-0.004820</td>
    </tr>
  </tbody>
</table>
<p>2507 rows × 7 columns</p>
</div>




```python
# Converting our strategy to dji
df_s["Strategy"] = df_s.Strategy_Ret.add(1, fill_value = 0).cumprod() * df_s.iloc[0,0]
df_s
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DJI_Close</th>
      <th>DJI_Return</th>
      <th>SMA20</th>
      <th>SMA50</th>
      <th>SMA200</th>
      <th>Position</th>
      <th>Strategy_Ret</th>
      <th>Strategy</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2010-10-19</td>
      <td>10978.620117</td>
      <td>-0.014813</td>
      <td>10920.518018</td>
      <td>10582.089805</td>
      <td>10507.098203</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>10978.620117</td>
    </tr>
    <tr>
      <td>2010-10-20</td>
      <td>11107.969727</td>
      <td>0.011782</td>
      <td>10938.951025</td>
      <td>10591.364199</td>
      <td>10509.777954</td>
      <td>1.0</td>
      <td>0.011782</td>
      <td>11107.969727</td>
    </tr>
    <tr>
      <td>2010-10-21</td>
      <td>11146.570312</td>
      <td>0.003475</td>
      <td>10963.158545</td>
      <td>10606.719004</td>
      <td>10512.642407</td>
      <td>1.0</td>
      <td>0.003475</td>
      <td>11146.570312</td>
    </tr>
    <tr>
      <td>2010-10-22</td>
      <td>11132.559570</td>
      <td>-0.001257</td>
      <td>10976.773535</td>
      <td>10622.971191</td>
      <td>10515.270903</td>
      <td>1.0</td>
      <td>-0.001257</td>
      <td>11132.559570</td>
    </tr>
    <tr>
      <td>2010-10-25</td>
      <td>11164.049805</td>
      <td>0.002829</td>
      <td>10994.374023</td>
      <td>10640.189180</td>
      <td>10518.000200</td>
      <td>1.0</td>
      <td>0.002829</td>
      <td>11164.049805</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2020-09-28</td>
      <td>27584.060547</td>
      <td>0.015092</td>
      <td>27779.850781</td>
      <td>27546.211953</td>
      <td>26277.101426</td>
      <td>1.0</td>
      <td>0.015092</td>
      <td>6144.422847</td>
    </tr>
    <tr>
      <td>2020-09-29</td>
      <td>27452.660156</td>
      <td>-0.004764</td>
      <td>27730.981250</td>
      <td>27561.647773</td>
      <td>26273.704473</td>
      <td>1.0</td>
      <td>-0.004764</td>
      <td>6115.153061</td>
    </tr>
    <tr>
      <td>2020-09-30</td>
      <td>27781.699219</td>
      <td>0.011986</td>
      <td>27687.783203</td>
      <td>27580.473750</td>
      <td>26271.936064</td>
      <td>1.0</td>
      <td>0.011986</td>
      <td>6188.447387</td>
    </tr>
    <tr>
      <td>2020-10-01</td>
      <td>27816.900391</td>
      <td>0.001267</td>
      <td>27623.603223</td>
      <td>27596.694961</td>
      <td>26269.841113</td>
      <td>1.0</td>
      <td>0.001267</td>
      <td>6196.288541</td>
    </tr>
    <tr>
      <td>2020-10-02</td>
      <td>27682.810547</td>
      <td>-0.004820</td>
      <td>27593.107227</td>
      <td>27617.304570</td>
      <td>26266.919365</td>
      <td>1.0</td>
      <td>-0.004820</td>
      <td>6166.419672</td>
    </tr>
  </tbody>
</table>
<p>2507 rows × 8 columns</p>
</div>




```python
# Plotting dji and our strategy
df_s[["DJI_Close", "Strategy"]].plot(figsize = (15,10), fontsize = 15)
plt.legend(fontsize = 15)
plt.title("SMA Strategy", fontsize = 20)
plt.show()
```


![png](output_97_0.png)



```python
# summary of returns and risk
summary_ann(df_s[["DJI_Return", "Strategy_Ret"]])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Return</th>
      <th>Risk</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DJI_Return</td>
      <td>0.106747</td>
      <td>0.174269</td>
    </tr>
    <tr>
      <td>Strategy_Ret</td>
      <td>-0.042587</td>
      <td>0.174350</td>
    </tr>
  </tbody>
</table>
</div>



* make functions


```python

```

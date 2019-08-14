# 收益率

#### 风险与回报

##### 收益率

- 简单收益率 (*Simple rate of return*)：*when dealing with multiple assets over the same timeframe*.

  $$
	简单收益率 = \frac{ 卖出价 - 买入价 }{ 买入价 }
  $$

- 对数收益率 (*Logarithmic rate of return*)：*when make calculations about a single asset over time*.
  $$
	对数收益率 = \log{\frac{卖出价}{买入价}} = \log{卖出价} -  \log{买入价}
  $$

> :pen: 核心就是对于单一投资品的收益率，对数收益率时序可加；对于不同投资品的截面收益率，应该用百分比收益率，因为它在截面上有可加性；另外对数收益率对建模有帮助。
>
> [参考说明](https://www.zhihu.com/question/30113132)

###### 简单收益率实例

```python
# 加载数据
import numpy as np
from pandas_datareader import data as wb
PG = wb.DataReader('PG', data_source='yahoo', start='1995-1-1')
# 计算历史收益率
PG['simple_return'] = (PG['Adj Close'] / PG['Adj Close'].shift(1)) - 1
print(PG['simple_return']) # print out as Text
```

用图表显示

```python
import matplotlib.pyplot as plt
PG['simple_return'].plot(figsize=(8, 5))
plt.show()
```

计算平均收益率

```python
avg_returns_d = PG['simple_return'].mean()
avg_returns_d # 结果是 0.00054655035334591408
# 计算年均收益率 
avg_returns_a = PG['simple_return'].mean() * 250
avg_returns_a # 结果是 0.13663758833647852
print(str(round(avg_returns_a, 5) * 100) + ' %')
# 结果是 13.664 %
```

###### 对数收益率实例

```python
PG['log_return'] = np.log(PG['Adj Close']/PG['Adj Close'].shift(1))
log_return_d = PG['log_return'].mean()*250
print(str(round(log_return_d, 5) * 100) + ' %' )
# 结果是 11.684999999999999 %
```

##### 收益横向比较

```python
tickers = ['PG', 'MSFT', 'F', 'GE']
mydata = pd.DataFrame()
for t in tickers:
    mydata[t] = wb.DataReader(t, data_source='yahoo', start='1995-1-1')['Adj Close']
```

`mydata`第一行的数据是 $mydata.iloc[0]$ . 效果等于 $mydata.loc['1995-01-03']$

> PG        6.441071
> MSFT   2.436452
> F           3.307851
> GE        4.087069
> Name: 1995-01-03 00:00:00, dtype: float64

以第一行为基准,计算历年来的增长, 并用图表打印出来.

```python
(mydata / mydata.iloc[0] * 100).plot(figsize = (15, 6));
plt.show()
```


![](https://github.com/maiernte/img/raw/master/python_finance/python经济四股收益对比曲线.png)

##### 加权收益

假设每股平均按25%的权重计算. 

```python
returns = (mydata / mydata.shift(1)) - 1
weights = np.array([0.25, 0.25, 0.25, 0.25])
np.dot(returns, weights)
```

> out: array([ nan,  0.0065397 , -0.0092297 , ...,  0.03227913, -0.00289308,  0.01411137]) 

计算历史以来平均年加权收益

```python
annual_returns = returns.mean() * 250
np.dot(annual_returns, weights)
# 结果是 0.14225289130269819
```

如果采用不同的加权, ` weights_2 = np.array([0.4, 0.4, 0.15, 0.05])`, 则结果为15.71% 



##### 指数收益

```python
tickers = ['^GSPC', '^IXIC', '^GDXI', '^FTSE']
```

> **S&P500**: ^GSPC, **NASDAQ**: ^IXIC, **DAX**: ^GDAXI, **London FTSE**: ^FTSE

###### 美国三大股指比较

```python
tickers = ['^GSPC', '^IXIC', '^GDAXI']
ind_data = pd.DataFrame()
for t in tickers:
    ind_data[t] = wb.DataReader(t, data_source='yahoo', start='1997-1-1')['Adj Close']
```

显示结果

```python
(ind_data / ind_data.iloc[0] * 100).plot(figsize=(15, 6));
plt.show()
```

![image-20190813185214650](https://github.com/maiernte/img/raw/master/python_finance/每股三大股指历史比较.png)

###### 个股与大盘指数比较

```python
tickers = ['PG', '^GSPC', '^DJI']
data_2 = pd.DataFrame()
for t in tickers:
    data_2[t] = wb.DataReader(t, data_source='yahoo', start='2007-1-1')['Adj Close']    
```

显示结果, PG 历年总体来说优于大盘. 

```python
(data_2 / data_2.iloc[0] * 100).plot(figsize=(15, 6));
plt.show()
```

![](https://github.com/maiernte/img/raw/master/python_finance/PG对比大盘.png)
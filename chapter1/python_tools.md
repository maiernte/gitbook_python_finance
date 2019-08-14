# 第一节: python 工具包

### python 作金融分析常用软件包

- [x] **numpy**  
- [x] **pandas**
- [x] **pandas_datareader**  `pip install pandas-datareader`
- [x] **math**
- [x] **random**
- [x] **quandl** 
- [x] **matplotlib** `pip install matplotlib`
- [ ] **statsmodels**   *descriptive statistics, plotting functions, regressions*

### 使用本地文件 .csv

#### 将数据存储为CSV文件

```python
import quandl
quandl.ApiConfig.api_key = "my quandl api key" # 否则每天只有50次匿名访问
mydata_01 = quandl.get("FRED/GDP")
mydata_01.to_csv('./data_02.csv')
```

#### 从CSV读取数据

```python
import pandas as pd
mydata_02 = pd.read_csv('./Data_02.csv')
mydata_02.head(10) # 显示前10行数据
```



#### 设置索引

**pandas** 会自动添加索引，我们也可以将数据列表中的某一列指定为索引，如

```python
# 方式 一
mydata_02 = pd.read_csv('./Data_02.csv', index_col='Date') 
# 方式 二
mydata_03 = pd.read_csv('./Data_03.csv')
mydata_03.set_index('Year')
```



### 使用在线API

#### 获取单个股票历史数据

```python
from pandas_datrareader import data as wb
PG = wb.DataReader('PG', data_source='yahoo', start='1995-1-1')
```

> **PG**: Procter & Gamble,  **AAPL**: Apple,  **MSFT**: Microsoft, **T**: AT&T, **F**: Ford, **GE**: General Electric
>
> 显示结果大概如下。如果不给出开始日期，大概会显示近十年的数据。

|       Date |      Open |      High |       Low |     Close |  Volume | Adj Close |      |
| ---------: | --------: | --------: | --------: | --------: | ------: | --------: | ---- |
| 1995-01-03 | 61.875000 | 62.500000 | 61.750000 | 62.375000 | 3318400 |  9.168104 |      |
|        ... |           |           |           |           |         |           |      |

2019 年8月份运行，数据源 `iex` 和 `morningstar` 都已经不能用了，获取不到数据。只有雅虎好像还能用。在执行数据调用的时候，如果jupyter左边显示 **In[\*]**，则表示等待结果。



#### 多个股票数据

下列代码获取五个公司的收盘价，并按列排在表格中。

```python
import pandas as pd
from pandas_datrareader import data as wb
tickers = ['PG', 'MSFT', 'T', 'F', 'GE']
new_data = pd.DataFrame()
for t in tickers:
  new_data[t] = wb.DataReader(t, data_source='yahoo', start='2015-1-1')['Adj Close']
new_data.tail()
```

:bell: 以上代码是**大小写敏感**的，比如在`iex`接口当中，就要写 *adj close*。



### 几个重要的数据接口

- **Quandl**：是为投资专业人士提供金融，经济和替代数据的首选平台，拥有海量的经济和金融数据。目前免费的就只有市场信息，个股的经济面信息。股价是要订阅收费的。
- **Rapid API**：估计跟Quandl差不多。
- **Tushare**：国内的数据接口，可以很方便获取A股和港股信息。
- **Yahoo finance**: 数据很多，也是免费的。但有时会中断服务，亚洲市场的数据也不够准确，特别是ETF。
- **IEX**：免费用户好像只能获取五年内的数据。
- **Alpha Vantage**：[官网](https://www.alphavantage.co/documentation/)，[使用示例](https://www.helplib.com/GitHub/article_154832) 免费的，而且各种数据都有。听说数据有限。但做每天的数据库更新，似乎是个不错的选项。



#### Quandl

[Quandl](https://www.quandl.com) 就好像金融數據的大賣場一樣, 既有海量的免費資產價格, 經濟, 財務數據, 也有眾多需要monthly subscription 的modules, 而這些基本都有詳細的documentation 去教導像我這類比較蹩腳的"碼農"去拿數據. 在下就比較喜歡收集聯儲局(Federal Reserve, **FED**) 的諸種數據, Quandl是很方便的選擇.



#### Tushare

隔了一道深圳河, 北面的金融科技比起這邊的香港完全是另外一個層次. 國內股票市場雖然還是封閉市場, 但是Data API 大數據科技就比香港開放多了不知多少倍. [Tushare](http://tushare.org) 只要一個non-gmail account (在下用outlook.com的email account 登記. btw 不知道tushare是否與google 有仇....) 做登記, 就可以獲取API token, 剛登記的用戶可以抓取 A股與B股的每日OHLC 數據, 對於會操作滬/深股通的港人來說都是有用的. Tushare 採取用戶積分制, 剛登記有100分, 每推薦一個新用戶加50分, 一般有500分以上才能看到個股基礎分析的信息, 有2000以上基本上除了每分鐘流量的限制以外, 甚麼都看得到.

[旧版文档](http://tushare.org/)

[TusharePro](https://tushare.pro)   [用户接口使用方式](https://tushare.pro/document/1?doc_id=40)




#### IEX

Investor Exchange(IEX) 2016年開始成為美國的其中一個受SEC認證的股票交易所, 他的賣點是股票上市頭五年免上市費用, Interactive Brokers 就是第一所從nasdaq轉移到IEX上市的股票. IEX 目前的API 1.0 可以用以下直接的HTTP request, 或者用**iexfinance (python) 或其他各種語言的unofficial libraries去拿取多項OHLC 或者財務數據. 用API 1.0 或者直接HTTP request 是不需要API key的.**

但是6月1日之後, 大部分API 1.0 可以拿取的數據, 將會轉移到IEX Cloud, 而IEX Cloud 是需要API key的, 免費的API key 每月可以獲取最多500,000條message, 此後月內每一百萬extra messages 要加US$1. 要筆者曾經用免費API key抓取AAPL 過去15年的OHLC daily data, 就已經用去了四萬條......

> *The IEX API will sunset all non-IEX data in June 2019. Data provided by IEX will continue to be offered in the IEX API for free.* **After June 1, 2019***, the following endpoints will continue to be offered by the IEX API, all other data from third-party providers will be available on* [IEX Cloud](https://www.iexcloud.io/)*, a new platform from IEX Group that is separate from the Exchange.*
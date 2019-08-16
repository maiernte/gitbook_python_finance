# 第六节: Alpha Vantage API

可以获取个股、汇率、加密币等数据。甚至可以进行指标运算，比如MACD、RSI等等。

python 模块从 Alpha Vantage API中获取股票数据/cryptocurrencies。

in提供了一个免费的API，用于实时财务数据和最常用的金融指示器，简单的json或者 Pandas 格式。 这个模块实现了 python Vantage ( http://www.alphavantage.co/ ) 提供的免费API的接口。 它需要一个免费的API，可以在 http://www.alphavantage.co/support/#api-key 请求。 你可以查看它们的文档 http://www.alphavantage.co/documentation 中的所有api调用。



## 代码示例

[参考文章](https://www.helplib.com/GitHub/article_154832)

[详细示例代码](https://github.com/RomelTorres/av_example/blob/master/Alpha%20vantage%20examples.ipynb)

### 安装

```shell
pip install alpha_vantage
# 从源代码安装
git clone https://github.com/RomelTorres/alpha_vantage.git
pip install -e alpha_vantage
```

### 用法

要获取 python 中的数据，只需导入库并使用api键调用对象，并准备一些出色的免费实时财务数据。 你的api键也可以存储在环境变量 `ALPHAVANTAGE_API_KEY` 中。

```python
from alpha_vantage.timeseries import TimeSeries
# Get json object with the intraday data and another with the call's 
ts = TimeSeries(key='YOUR_API_KEY') 
metadatadata, meta_data = ts.get_intraday('GOOGL')
```

内部有重试计数器，可以用于最小化连接错误( 如果api不能及时响应的话)，默认设置为 **5** 但可以随时增加或者减少。`ts = TimeSeries(key='YOUR_API_KEY',retries='YOUR_RETRIES')`

库支持将它的结果作为json字典( 默认值)。Pandas dataframe ( 如果已经安装) 或者 csv，只需传递参数 `output_format='Pandas'` 来更改给定类的输出格式。 请注意，有些API调用不支持csv格式( 即 ***ForeignExchange, SectorPerformances and TechIndicators*** ) 因为API终结点不支持它的调用中的格式。

### 数据帧结构

数据帧结构是通过对 alpha vantage REST API的调用给出的。 数据框架的列名是由它们的数据结构给出的。 例如下面的调用：

```python
from alpha_vantage.timeseries import TimeSeriesfrom pprint import pprint
ts = TimeSeries(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = ts.get_intraday(symbol='MSFT',interval='1min', outputsize='full')
pprint(data.head(2))
```

### 打印

#### 时间序列

使用 Pandas 支持，我们可以很容易地对'msft'股票的帧内值进行 plot:

```python
from alpha_vantage.timeseries import TimeSeriesimport matplotlib.pyplot as plt
ts = TimeSeries(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = ts.get_intraday(symbol='MSFT',interval='1min', outputsize='full')
data['4. close'].plot()
plt.title('Intraday Times Series for the MSFT stock (1 min)')
plt.show()
```

#### 技术指标

```python
from alpha_vantage.techindicators import TechIndicatorsimport matplotlib.pyplot as plt
ti = TechIndicators(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = ti.get_bbands(symbol='MSFT', interval='60min', time_period=60)
data.plot()
plt.title('BBbands indicator for MSFT stock (60 min)')
plt.show()
```

#### 扇区性能

我们也可以轻松地实现 plot 扇区的性能：

```python
from alpha_vantage.sectorperformance import SectorPerformancesimport matplotlib.pyplot as plt
sp = SectorPerformances(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = sp.get_sector()
data['Rank A: Real-Time Performance'].plot(kind='bar')
plt.title('Real Time Performance (%) per Sector')
plt.tight_layout()
plt.grid()
plt.show()
```

### 货币

#### 加密币

```python
from alpha_vantage.cryptocurrencies import CryptoCurrenciesimport matplotlib.pyplot as plt
cc = CryptoCurrencies(key='YOUR_API_KEY', output_format='pandas')
data, meta_data = cc.get_digital_currency_intraday(symbol='BTC', market='CNY')
data['1b. price (USD)'].plot()
plt.tight_layout()
plt.title('Intraday value for bitcoin (BTC)')
plt.grid()
plt.show()
```

#### 外汇

外部exchange只是元数据，因这里只有( 使用'格式的)'或者或或者'Pandas"格式会引发错误。

```python
import alpha_vantage.foreignexchange import ForeignExchangefrom pprint import pprint
# There is no metadata in this calldata, _ =
cc = ForeignExchange(key='YOUR_API_KEY')  
cc.get_currency_exchange_rate(from_currency='BTC',to_currency='USD')
pprint(data)
```
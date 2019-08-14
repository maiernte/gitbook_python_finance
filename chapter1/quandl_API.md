# 第三节: Quandl API

## Quandl API

Quandl是为投资专业人士提供金融，经济和替代数据的首选平台，拥有海量的经济和金融数据。Python有Quandl 模块，通过Quandl模块可直接使用平台上的数据。Quandl包可以访问平台上所有免费的数据，但不是所有的数据都是免费的，部分数据需要付费才能使用。



Quandl 就好像金融數據的大賣場一樣, 既有海量的免費資產價格, 經濟, 財務數據, 也有眾多需要monthly subscription 的modules, 而這些基本都有詳細的documentation 去教導像我這類比較蹩腳的"碼農"去拿數據. 在下就比較喜歡收集聯儲局(Federal Reserve, **FED**) 的諸種數據, Quandl是很方便的選擇.



## 安装以及认证

### 安装

quandl在python2和python3都可以安装。

```python
pip install quandl
```

### 认证 

虽然Quandl可以免费使用，但是下载数据或一天调用超过50次就需要身份认证。要想获得认证，需要先在[官网](https://link.zhihu.com/?target=https%3A//www.quandl.com/)上注册账号，然后获得API key，在使用中加入如下代码：

```python
# ㊙️ quandl.ApiConfig.api_key = "6FiQg8nNgWgUJs2csZHS"
quandl.ApiConfig.api_key = "YourAPIKEY" #YOURAPIKEY换成你的API key
```

### 使用

[Quandl快速入门](https://zhuanlan.zhihu.com/p/41063833)

代码示例：

```python
import quandl
quandl.ApiConfig.api_key = "xxxxxxxxxxxx"
start = datetime.datetime(2019,1,1)
end = datetime.date.today()
apple = andl.get("FINRA/FNSQ_AAPL",start_date=start, end_date=end)
```

上面只返回了证券市场的每日成交量。当希望获取股价，`WIKI/AAPL` 的时候，返回了空值。进入Quandl网站，查找“AAPL”时，才发现`free`的数据没有包括在内。所以，以后Quandl只能用来获取经济面的数据，比如GDP、消费指数之类的。
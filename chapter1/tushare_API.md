# 第四节: Tushare API

## 快速入门

[旧版文档](http://tushare.org/)

[TusharePro](https://tushare.pro)   

 [用户接口使用方式](https://tushare.pro/document/1?doc_id=40)

安装 Tushare 工具包

```sh
pip install tushare
pip install tushare -- upgrade # 版本升级
```

在官网注册并获取 tocken , 然后就可以在 py 文件中使用

### 读取A股证券代号

```python
import tushare
print(tushare.__version__)
# 1.2.40
ts.set_token('my_tocken')
pro = ts.pro_api()
# 获取市场中所有股票的代码
data = pro.stock_basic(exchange='', list_status='L', fields='ts_code,symbol,name,area,industry,list_date') 
data.head()
```

![](https://github.com/maiernte/img/raw/master/python_finance/A股证券代号.png)

### 读取个股历史数据

```python
shenhuo = ts.get_hist_data('000933')
shenhuo.tail()
```

![](https://github.com/maiernte/img/raw/master/python_finance/000933_history.png)

没有设定起始日期， 但结果显示只有一年多的数据。根据官网的说明，每次只返回500条记录。因此如果要读取个股历史上所有的数据，只能分几次进行了。
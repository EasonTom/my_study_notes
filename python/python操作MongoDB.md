# Python操作MongoDB

## 1 安装包

```shell
pip install pymongo
```

## 2 建立连接

```Python
import pymongo

# 建立连接，传入ip地址和端口，MongoDB默认端口是27017
client = pymongo.MongoClient('127.0.0.1', 27017)

# 获取数据库
db = client['mydb']
# 获取集合
col = db['student']
```

## 3 操作

```
# 取出一条
data = col.find_one() # 随机取出一条
data = col.find_one({'name': 'xiaoming'}) # 指定取出

# 插入数据
data = col.insert_one(dict) # 插入一条
data = col.insert_many([dict1, dict2]) # 插入多条

# 更新
data = col.update_one({'name':'aa'},{'$set':{'name':'bb'}}) # 更新一条，指定更新，必须要有"$"
data = col.update_many({'name':'aa'},{'$set':{'name':'bb'}}) # 插入多条

# 删除
delete_one()
delete_many()
```


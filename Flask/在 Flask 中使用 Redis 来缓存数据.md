## （一） Redis 简介
Redis 是一个高性能的 key-value 数据库，它是基于内存运行的数据库，因此有很高的性能，存取速度非常快，而且 Redis 还可以定期的将数据同步到磁盘中，实现数据的持久化。

Redis 的应用场景：
- 登录会话存储
- 排行榜/计数器，比如文章阅读数、点赞数
- 作为消息队列
- 当前在线人数统计
- 常用数据的缓存，减少数据库访问压力

## （二） Redis 的安装
redis 的安装请参照 [安装 Redis](https://redis.com.cn/redis-installation.html#%E5%9C%A8-windows-%E4%B8%8A%E5%AE%89%E8%A3%85-redis-)

## （三） 在 Flask 中使用 Redis 来缓存数据
### 1. 在 Flask 中使用 Redis，需要先安装 flask-redis 包
```
pip install flask-redis
```

### 2. flask-redis 的配置
```
REDIS_URL = 'redis://:password@localhost:6379/0'
```

### 3. flask-redis 的初始化
```
redis_client = FlaskRedis(app)
或
redis_client = FlaskRedis()
redis_client.init_app(app)
```

### 4. 完整的代码示例
```py
from flask import Flask, jsonify
from flask_redis import FlaskRedis
import time


app = Flask(__name__)


# flask-redis 的配置和初始化
app.config['REDIS_URL'] = 'redis://:123456@localhost:6379/0'
redis_client = FlaskRedis(app)


@app.route('/')
def index():
    return 'Hello World'

@app.route('/redis/set_data/<int:id>')
def set_data(id):
    # 准备相关的数据
    user_id = str(id)
    data = 'dyn_data_{}'.format(user_id)
    data_key = 'dyn_key_{}'.format(user_id)
    # 设置超时时间为 60 秒，当动态数据超过 60 没有更新时，Redis 会自动清除该数据。
    expires = int (time.time()) + 60

    # 写入 redis 中
    # 通过管道 pipeline 来操作 redis，以减少客户端与 redis-server 的交互次数。
    p = redis_client.pipeline()
    p.set(data_key, data)
    p.expireat(data_key, expires)
    p.execute()

    return '设置成功'

@app.route('/redis/get_data/<int:id>')
def get_data(id):
    user_id = str(id)
    data_key = 'dyn_key_{}'.format(user_id)
    data = redis_client.get(data_key)

    print('data = {}'.format(data))

    if data:
        return jsonify(
            {
                'data': data.decode(),
            }
        )
    else:
        return jsonify({})
```

### 5. 效果图
使用可视化工具 [AnotherRedisDesktopManager](https://github.com/qishibo/AnotherRedisDesktopManager/) 来查看 Redis 中的状态

- 设置缓存数据
![](在%20Flask%20中使用%20Redis%20来缓存数据.assets/访问链接并保存数据.png)
![](在%20Flask%20中使用%20Redis%20来缓存数据.assets/可视化工具查看redis中保存的数据.png)
- 获取缓存数据
注意：由于设置了超时时间为 60 秒，当动态数据超过 60 没有更新时，Redis 会自动清除该数据。
![](在%20Flask%20中使用%20Redis%20来缓存数据.assets/网页获取redis中保存的数据.png)


## （四） 参考
[Flask 使用 Redis 存储动态数据](https://zhuanlan.zhihu.com/p/140511906)

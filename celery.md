## celery
## http://docs.jinkan.org/docs/celery/getting-started/brokers/index.html
### 使用redis作为broker

* 安装redis: pip install -U celery[redis]
* 配置中间人: BROKER_URL =''redis://:password@hostname:port/db_number
* 设置超时时间：BROKER_TRANSPORT_OPTIONS = {'visibility_timeout': 3600}
* 配置backend，用来保存结果：CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
* 在tasks.py中配置：

> app = Celery('tasks', backend='redis://localhost', broker='amqp://')

### 使用celey
* 创建一个简单的celey实例
` from celery import Celery`

`app = Celery('tasks', broker='amqp://guest@localhost//')`

`@app.task`

`def add(x, y):`
	
`return x + y`

* 执行程序

`celery -A tasks worker --loglevel=info`


* 调用celery任务：

> from tasks import add

> result = add.delay(4,4)

> result.backend 为结果
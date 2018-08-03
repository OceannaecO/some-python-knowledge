## 模块
### json：
*	import json 
*	json数据-->python对象: json.loads()
*	python对象-->json数据：json.dumps()
*	从网络请求中获取json类型数据： response.json()
*	发送请求的时候如果需要发送的数据是json的话： requests.post(url, json=?)
*	常见问题：json中数据格式必须是双引号开头

### BSON：
* 是一种基于json的数据类型，是mongodb的专用格式

##requests: --一个类似的方法 urllib2
*	发送带参数的get请求：requests.get(url, params, headers, auth) 

*	post请求传参数用data
*	传文件用files = {'file': open('/home/lyb/sjzl.mpg', 'rb')}, cookies, 设置请求的超时时间timeout
*	设置代理proxies = {
																	  "http": "http://10.10.1.10:3128",
																	  "https": "http://10.10.1.10:1080",
																	}

*	常见命令：response = requests.get(url, params:传参数, headers:传请求头) 
>			response.status_code--查看响应码
>			response.headers['key']--查看响应头
>			response.content/text/json()--查看响应体
>			response.cookies['key']--获取cookie值

### asyncio：https://github.com/aio-libs/aiohttp


### gevent:
* 	gevent的使用：
* 	`from gevent import monkey, Timeout`
	`import gevent`
1. timeout:用来设置每个gevent任务的超时时间，在py文件开头使用：
	`timeout = Timeout(1000)
	 timeout.start()`
2. monkey:用来改变全局的堵塞方式，能够被gevent所识别,在py文件开头使用：
	`monkey.patch_socket()`
3. 同时创建多个gevent任务：
	`gevent.joinall([
        gevent.spawn(func, args) for args in args
    ])`
4. 创建单个任务：
	`gevent.spawn(func, args)`

### marshmallow
*	作用：对一个字典类型的数据进行筛选和判断
*	用法：1.导入包
>		rom marshanllow import Schema, fields, validates, ValidationError, validates_schema
*		2.定义一个类
>			 class JobSchema(Schema):
			 	url = fields.Url(required=True) (还有fields.List,String,Boolean,Int...)
			 	weight = fields.String(required=True：表示这个字段不能为none)
				@validates('weight')
				'''对weight的值进行判定'''
				def validate_weight(self, value)
					if value not in ['normal', 'high', 'very-high']:
						raise ValidationError('...')
*		3.使用
>			     import JobSchema 
>			     job = JobSchema().load(需要判断的东西)
>			     job.data--job的内容
>			     job.errors--job的错误

##工具

###docker:

###git:
	基本命令：
	1.创建一个文件夹后，git init
	2.将文件添加到版本库的暂存区， git add ***
	3.将文件提交到版本库的当前分支， git commmit -m '这里是注释'
	4.查询状态： git status   单词：modify(修改）Untracked（未被发现的）
	5.查询具体修改内容： git diff 文件名
	6.查看提交日志：git log  (--pretty=oneline:可以让结果更精简)
	版本回退：HEAD:表示当前版本 HEAD^：表示上个版本，以此类推  HEAD~10:表示上10个版本
	1.git reset --hard HEAD?：这里也可以是版本号   (如果反悔了要重新回到最新版本，请记得版本号)
	2.git reflog 记录了每一次命令
	3.git checkout -- 文件名 撤销修改 未添加到暂存区（回到版本库状态），添加到暂存区（回到添加到暂存区后的状态），如果没有--则是切换分支的命令
	4.git reset HEAD 文件名   版本回退，也可以把暂存区的修改回退到工作区
	5.删除文件：rm 文件名————》  git rm 文件名————》git commit -m '***'
	远程仓库：
	1.git push -u origin master   -u 用作将本地分支和远程分支关联起来  之后提交就可以不加-u了
	2.查看远程仓库的信息：git remote
	分支管理：
	1.git checkout -b 分支名  创建并切换分支，也可以分开（git branch 分支名--》git checkout 分支名）
	2.查看当前分支：git branch
	3.合并指定分支到当前分支：git merge 分支名   也可以加上-m参数 
	4.删除分支：git branch -d 分支名   如果分支未合并就要删除：—D
	5.查看合并分支图：git log --graph
	6.储藏分支：git stash    git stash list(查看储藏列表)  git stash pop(恢复且删除),也可以分步：git stash apply stash@{0}    git stash drop
	7.拉取远程分支到本地：git checkout -b dev origin/分支名
	解决冲突：
	1.先把最新代码pull到本地，手动处理后再push
	标签：
	1.创建标签：git tag -a 标签名字 -m 说明信息（默认给最新的commit打上标签） 后面也可以接commit id，指定
	2.查看标签：git tag
	3.查看标签信息：git show 标签名 






###scrapy:
1.	scrapy_splash:用来对动态请求的网站进行处理,需要docker来协同工作
`{
		from scrapy_splash import SplashRequest
		yeild SplashRequest(
			url,
			callback_function,
			args={},
			meta
		)
	}`
	


###flask:
* flask接受请求参数：`from flask import request`
						 ` value = request.args.get('value')`
						  将请求的参数转化为字典：`value = requests.args.to_dict()`
*						  request.json--提取出请求中的json对象，可以使用get方法获取指定key的值

## python

###python:
* 	isinstance: isinstance(内容, list,dict...) 
*	数据库多次连接的消耗问题：创建一个永久的连接对象，在需要对数据库操作时调用这个连接对象即可
*	pop: dict.pop('key')--删除字典中key和value

## 数据库
### mongodb && pymongo:
*	import pymongo
*	创建链接：client = pymongo.MongoClient('localhost', 27017)--链接本地
*		client = pymongo.MongoClient('mongodb://localhost:27017')--链接指定
*	连接数据库：db = client.数据库的名字 or db = client['数据库的名字']
*	插入文档：insert_one(value) or insert_many([value1, value2])
*	检索文档：db.collections.find_one({'key':'value'}, ['key', 'key'])--前面字典表示给定筛选条件，后面的列表表示给定展示结果范围。
*			 db.collections.find({},[])--查找多条记录，返回一个可迭代对象
*	查询条件：
1.   $ne--不等 $gt--大于 $lt--小于 $lte--小等 $gte--大等 
2.			 {'key':{'$ne':0}}, 或者{'key':'a & b'}， 指定范围{'key':{'$gt':50, '$lt':100}}
3.			 $or:{'$or':[{条件},{条件}]}，
4.			 $in,$nin,$all:{'key':{'$in':[100,200]}}
5.		 匹配字段不存在：{'key':None}, 存在：{'key':{'$ne':None}}
6.		 正则匹配：{'key':{'$regex','正则'}}
7.		 统计：count()
8.			 排序：sort()
9.		 限制数量：limit()
10.   	添加记录：单条插入：db.collections_name.insert_one(字典类型的数据)
11.			 多条插入：db.collections_name.insert_many([value1, value2])


###mysql:
1.	import pymysql
2.	创建连接实例：conn = pymysql.connect(hose, user, passwd, db)
3.	Conneciton命令：
4.	提交当前事物：conn.commit()
5.	回滚当前事物：conn.rollback()
6.	关闭连接：conn.close()
7.	创建光标对象：cur = conn.cursor()
8.	Cursor命令：
9.	执行一行查询命令：cur.execute()
10.	获取结果集中的内容：fetchone()--获取一个 fetchmany(size)--获取size行  fetchall()--获取全部
11.	关闭游标：cur.close()




### redis： 
1.	import redis
2.	创建redis实例：client = redis.Redis(host=?, port=?, db=?, decode_response=True)
3.	往redis里面插入数据：
4.		集合： client.set(collection_name, json.dumps(data))
5.	查询操作：client.keys--查看所有keys
			   client.dbsize--查看有多少keys
			   client.delete('key')--删除key
6.	采用pipeline一次性提交代码：
			p = client.pipeline()--创建
			p.execute--提交
7.	消息队列：task = client.blpop(collection_name, 0)[1]--堵塞等待从collection里取值
8.	订阅模式：TODO




### xpath:
*	|: 相当于或者的意思
*	contains: //div[contains(@class, '内容')]

## 环境
### mac开发环境的安装：
	brew: 包管理工具{
		安装：ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
		安装好后可以修改源：cd "$(brew --repo)"
						 git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
						 echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
						 source ~/.bash_profile
		命令：brew list 已经下载好的包
			 brew update 更新brew
			 brew install 使用brew下载。。。
	}
	pyenv(一个python的版本管理工具)：{
		安装：brew install pyenv
		配置： echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
			  echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
			  echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
			 重启终端
	 	安装python版本：pyenv install 3.6.4
	 	命令：pyenv global  选择全局的python版本
	}
	pyenv-virtualenv(用来创建和管理python虚拟环境)：{
		安装： brew install pyenv-virtualenv
		在环境配置文件中加上：
					eval "$(pyenv init -)"
					eval "$(pyenv virtualenv-init -)"
		创建虚拟环境：pyenv virtualenv 2.7.10 virtualenv-name
		使用虚拟环境：pyenv active virtualenv-name
		查看环境：pyenv version 
		退出虚拟环境：pyenv deactivate
	}
	其他软件包都可以用brew来安装管理，有些配置问题可以去github或者官网查看，其中mysql,redis,mongodb中只有mongodb的安装出现了一点小问题，就是需要创建一个mongodb的默认数据存储位置：sudo mkdir -p /data/db，以后要是还有什么问题我也会及时更新的。



### 环境变量：
	不同形式的终端设置环境变量的地方：mac（~/bash_profile）ubuntu（~/.bashrc） zsh（~/.zshrc）
	添加一个环境变量：export ENV=test
	添加一个路径：export SSH_KEY_PATH="~/.ssh/rsa_id"
	在代码中获取环境变量的值：import os  
						   name = os.getenv('name')




### crontab: 
		查看crontab的执行情况：tail -f /var/log/cron
		重点问题：crontab 无法获取环境变量，所以在python中如果设置了环境变量会出现导入错误。
		在crontab定时任务的命令前面加上 source ~/.bash_profile  应该可以解决这个问题
		
### centos 上配置环境以及部署 
1.   https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-centos-7

2.   https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-centos-7

3.   https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-centos-7
### python 相关网站
1. 一个关于大牛的python规范：https://docs.python-guide.org/

2. 代码风格：https://pythonguidecn.readthedocs.io/zh/latest/writing/style.html







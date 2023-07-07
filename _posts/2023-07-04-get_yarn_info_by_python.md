---
layout: post
title:  "开发|使用python脚本定时请求yarn集群的资源信息"
date:   2023-07-04 16:12:33+0800
categories: work
tags: python dev bigdata log crontab
author: Bumon
mathjax: true
---

* content
{:toc}

接到一个获取yarn集群信息的需求，本来直接请求就搞定，但因为一些权限和管理的缘故，接口不能暴露出去，遂使用python定时请求接口，并把数据落到mysql中。不难，也没遇到什么问题，本着blogs弄都弄了的想法，简单记录一下🤣





## 实现思路

通过python请求yarn集群webui的接口，获取当前集群资源状态，然后直接落到mysql里供web方调用数据做可视化等后续操作。定时的调度选择用部署脚本服务器里的`crontab` 来实现。

## 遇到的小问题


### 缺少所需要的包/cron调用不到conda环境
因为机器上装了anaconda，调试代码时候环境是ok的，但配置到cron里就遇到问题了，用python调用的是机器默认的python环境。本来想直接给python装包，结果pip调用的都是conda里的pip，小需求不想花太多时间，最后通过在cron里使用绝对路径解决。（好吧这个根本不算个问题hhh）

`corntab -e` 进入crontab配置，贴入下列调度代码，目前的需求是5分钟查询一次,并且为了有迹可循输出了一份日志：
```text
*/5 * * * * /xxxx/apps/anaconda3/bin/python3 /home/oozie/cron_script/hadoop_info_2mysql.py >> /home/oozie/cron_script/logs/hadoop_info_2mysql.log 2>&1
```

## 实现代码

简单贴一贴
```python
#!/xxx/apps/anaconda3
# -*- coding: utf-8 -*-
import requests
import json
import mysql.connector
import datetime
# yarn的接口url
url='http://url:port/ws/v1/cluster/metrics'
# response = requests.get(url)#,proxies=proxy)

# 连接 MySQL 数据库


# 正式环境
mySqldb = mysql.connector.connect(
    host='ip.ip.ip.ip',
    user='yourUser',
    password='yourPassword',
    database='yourdb'
)
dt = datetime.date.today().strftime('%Y-%m-%d')


with requests.get(url) as r:
    if r.status_code != 200:
        print("error")
    else:
        json_str = json.dumps(r.text)
        json_tmp=json.loads(json_str)
        json_tmp2=json.loads(json_tmp)
        json_data=json_tmp2["clusterMetrics"]
        json_data['dt']="'"+dt+"'"
        # 获取游标
        mycursor = mySqldb.cursor()
        # 定义 SQL 语句并执行
        sql = "INSERT INTO yourTable (%s) VALUES (%s)"
        cols = ", ".join('`{}`'.format(k) for k in json_data.keys())
        val_cols = ', '.join('{}'.format(k) for k in json_data.values())
        res_sql=sql % (cols,val_cols)
        mycursor.execute(res_sql)
        # 提交更改
        # print(res_sql)
        mySqldb.commit()




```

## 参考链接


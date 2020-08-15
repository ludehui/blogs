---
published: true
title: python之logging日志模块
category: logging
tags: 
  - python
  - logging
layout: post
---







# python之logging日志模块

## 1.logging模块简介

用Python写代码的时候，在想看的地方写个print xx 就能在控制台上显示打印信息，这样子就能知道它是什么了，但是当我需要看大量的地方或者在一个文件中查看的时候，这时候print就不大方便了，所以Python引入了logging模块来记录我想要的信息。print也可以输入日志，logging相对print来说更好控制输出在哪个地方，怎么输出及控制消息级别来过滤掉那些不需要的信息。logging模块是Python内置的标准模块，主要用于输出运行日志，可以设置输出日志的等级、日志保存路径、日志文件回滚等；相比print，具备如下优点：

- 可以通过设置不同的日志等级，在release版本中只输出重要信息，而不必显示大量的调试信息；
- print将所有信息都输出到标准输出中，严重影响开发者从标准输出中查看其它数据；logging则可以由开发者决定将信息输出到什么地方，以及怎么输出；

## 2.日志等级

logging日志一共分为5个等级，它们分别是：

- CRITICAL：打印critical级别,一个严重的错误,这表明程序本身可能无法继续运行；
- ERROR：打印error,critical级别的日志,更严重的问题,软件没能执行一些功能；
- WARNING：打印warning,error,critical级别的日志,一个迹象表明,一些意想不到的事情发生了,或表明一些问题在不久的将来(例如。磁盘空间低”),这个软件还能按预期工作；
- INFO：打印info,warning,error,critical级别的日志,确认一切按预期运行；
- DEBUG：打印全部的日志,详细的信息,通常只出现在诊断问题上。

从优先级来看，它们的排序为：

```
CRITICAL > ERROR > WARNING > INFO > DEBUG
```

不同等级日志使用范围：

- FATAL：致命错误
- CRITICAL：特别糟糕的事情，如内存耗尽、磁盘空间为空，一般很少使用 
- ERROR：发生错误时，如IO操作失败或者连接问题
- WARNING：发生很重要的事件，但是并不是错误时，如用户登录密码错误 
- INFO：处理请求或者状态变化等日常事务 
- DEBUG：调试过程中使用DEBUG等级，如算法中每个循环的中间状态

## 3.部分名词解释

- Logging.Formatter：这个类配置了日志的格式，在里面自定义设置日期和时间，输出日志的时候将会按照设置的格式显示内容。
- Logging.Logger：Logger是Logging模块的主体，进行以下三项工作：
  - \1. 为程序提供记录日志的接口
  - \2. 判断日志所处级别，并判断是否要过滤
  - \3. 根据其日志级别将该条日志分发给不同handler
- 常用函数有：
  - Logger.setLevel() 设置日志级别
  - Logger.addHandler() 和 Logger.removeHandler() 添加和删除一个Handler
  - Logger.addFilter() 添加一个Filter,过滤作用
  - Logging.Handler：Handler基于日志级别对日志进行分发，如设置为WARNING级别的Handler只会处理WARNING及以上级别的日志。
- 常用函数有：
  - setLevel() 设置级别
  - setFormatter() 设置Formatter

## 4.基本使用

配置logging基本的设置，然后在控制台输出日志，代码如下：

```
import logging
logging.basicConfig(level = logging.WARNING,format = '%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

logger.info("郭富城")
logger.debug("黎明")
logger.warning("张学友")
logger.info("刘德华")
```

运行时输出：

```
2020-07-30 11:20:17,690 - __main__ - WARNING - 张学友
```

如果把代码第二行的日志输出等级改变为DEBUG，则输出为：

```
2020-07-30 11:23:17,306 - __main__ - INFO - 郭富城
2020-07-30 11:23:17,306 - __main__ - DEBUG - 黎明
2020-07-30 11:23:17,306 - __main__ - WARNING - 张学友
2020-07-30 11:23:17,306 - __main__ - INFO - 刘德华
```

其中，还使用了一些参数，

```
%(asctime)s：打印日志的时间
%()s：__main__
%(levelname)s：打印日志级别的名称
%(message)s：打印日志信息
```

除此之外，还有：

```
%(levelno)s：打印日志级别的数值
%(pathname)s：打印当前执行程序的路径，其实就是sys.argv[0]
%(filename)s：打印当前执行程序名
%(funcName)s：打印日志的当前函数
%(lineno)d：打印日志的当前行号
%(thread)d：打印线程ID
%(threadName)s：打印线程名称
%(process)d：打印进程ID
```

## 5.将日志写入到文件

```
import logging
logger = logging.getLogger(__name__)
logger.setLevel(level = logging.DEBUG)
handler = logging.FileHandler("log.txt")
handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)
logger.addHandler(console)

logger.info("郭富城")
logger.debug("黎明")
logger.warning("张学友")
logger.info("刘德华")
```

log.txt中的输出为：

```
2020-07-30 11:33:36,183 - __main__ - INFO - 郭富城
2020-07-30 11:33:36,183 - __main__ - WARNING - 张学友
2020-07-30 11:33:36,183 - __main__ - INFO - 刘德华
```

可以发现，logging有一个日志处理的主对象，其他处理方式都是通过addHandler添加进去，logging中包含的handler主要有如下几种：

```
#handler名称：位置；作用

StreamHandler：logging.StreamHandler；日志输出到流，可以是sys.stderr，sys.stdout或者文件
FileHandler：logging.FileHandler；日志输出到文件
BaseRotatingHandler：logging.handlers.BaseRotatingHandler；基本的日志回滚方式
RotatingHandler：logging.handlers.RotatingHandler；日志回滚方式，支持日志文件最大数量和日志文件回滚
TimeRotatingHandler：logging.handlers.TimeRotatingHandler；日志回滚方式，在一定时间区域内回滚日志文件
SocketHandler：logging.handlers.SocketHandler；远程输出日志到TCP/IP sockets
DatagramHandler：logging.handlers.DatagramHandler；远程输出日志到UDP sockets
SMTPHandler：logging.handlers.SMTPHandler；远程输出日志到邮件地址
SysLogHandler：logging.handlers.SysLogHandler；日志输出到syslog
NTEventLogHandler：logging.handlers.NTEventLogHandler；远程输出日志到Windows NT/2000/XP的事件日志
MemoryHandler：logging.handlers.MemoryHandler；日志输出到内存中的指定buffer
HTTPHandler：logging.handlers.HTTPHandler；通过"GET"或者"POST"远程输出到HTTP服务器
```

## 6.多模块使用logging

- 主模块

```
#mainModule.py
import logging
import subModule
logger = logging.getLogger("mainModule")
logger.setLevel(level = logging.INFO)
handler = logging.FileHandler("log.txt")
handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)

console = logging.StreamHandler()
console.setLevel(logging.INFO)
console.setFormatter(formatter)

logger.addHandler(handler)
logger.addHandler(console)


logger.info("creating an instance of subModule.subModuleClass")
a = subModule.SubModuleClass()
logger.info("calling subModule.subModuleClass.doSomething")
a.doSomething()
logger.info("done with  subModule.subModuleClass.doSomething")
logger.info("calling subModule.some_function")
subModule.som_function()
logger.info("done with subModule.some_function")
```

- 子模块

```
import logging

module_logger = logging.getLogger("mainModule.sub")
class SubModuleClass(object):
    def __init__(self):
        self.logger = logging.getLogger("mainModule.sub.module")
        self.logger.info("creating an instance in SubModuleClass")
    def doSomething(self):
        self.logger.info("do something in SubModule")
        a = []
        a.append(1)
        self.logger.debug("list a = " + str(a))
        self.logger.info("finish something in SubModuleClass")

def som_function():
    module_logger.info("call function some_function")
```

执行时候，我们可以在log.txt中看到以下内容：

```
2020-07-30 11:40:16,506 - mainModule - INFO - creating an instance of subModule.subModuleClass
2020-07-30 11:40:16,506 - mainModule.sub.module - INFO - creating an instance in SubModuleClass
2020-07-30 11:40:16,506 - mainModule - INFO - calling subModule.subModuleClass.doSomething
2020-07-30 11:40:16,506 - mainModule.sub.module - INFO - do something in SubModule
2020-07-30 11:40:16,506 - mainModule.sub.module - INFO - finish something in SubModuleClass
2020-07-30 11:40:16,506 - mainModule - INFO - done with  subModule.subModuleClass.doSomething
2020-07-30 11:40:16,506 - mainModule - INFO - calling subModule.some_function
2020-07-30 11:40:16,506 - mainModule.sub - INFO - call function some_function
2020-07-30 11:40:16,506 - mainModule - INFO - done with subModule.some_function
```

首先在主模块定义了logger'mainModule'，并对它进行了配置，就可以在解释器进程里面的其他地方通过getLogger('mainModule')得到的对象都是一样的，不需要重新配置，可以直接使用。定义的该logger的子logger，都可以共享父logger的定义和配置，所谓的父子logger是通过命名来识别，任意以'mainModule'开头的logger都是它的子logger，例如'mainModule.sub'。

实际开发一个application，首先可以通过logging配置文件编写好这个application所对应的配置，可以生成一个根logger，如'PythonAPP'，然后在主函数中通过fileConfig加载logging配置，接着在application的其他地方、不同的模块中，可以使用根logger的子logger，如'PythonAPP.Core'，'PythonAPP.Web'来进行log，而不需要反复的定义和配置各个模块的logger。

## 7.通过JSON或者YAML文件配置logging模块

尽管可以在Python代码中配置logging，但是这样并不够灵活，最好的方法是使用一个配置文件来配置。以从字典中加载logging配置，也就意味着可以通过JSON或者YAML文件加载日志的配置。

- json配置文件

```
{
    "version":1,
    "disable_existing_loggers":false,
    "formatters":{
        "simple":{
            "format":"%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        }
    },
    "handlers":{
        "console":{
            "class":"logging.StreamHandler",
            "level":"DEBUG",
            "formatter":"simple",
            "stream":"ext://sys.stdout"
        },
        "info_file_handler":{
            "class":"logging.handlers.RotatingFileHandler",
            "level":"INFO",
            "formatter":"simple",
            "filename":"info.log",
            "maxBytes":"10485760",
            "backupCount":20,
            "encoding":"utf8"
        },
        "error_file_handler":{
            "class":"logging.handlers.RotatingFileHandler",
            "level":"ERROR",
            "formatter":"simple",
            "filename":"errors.log",
            "maxBytes":10485760,
            "backupCount":20,
            "encoding":"utf8"
        }
    },
    "loggers":{
        "my_module":{
            "level":"ERROR",
            "handlers":["info_file_handler"],
            "propagate":"no"
        }
    },
    "root":{
        "level":"INFO",
        "handlers":["console","info_file_handler","error_file_handler"]
    }
}
```

通过加载json文件，，然后使用logging.dictConfig来配置logging。

```
import json
import logging.config
import os

def setup_logging(default_path = "logging.json",default_level = logging.INFO,env_key = "LOG_CFG"):
    path = default_path
    value = os.getenv(env_key,None)
    if value:
        path = value
    if os.path.exists(path):
        with open(path,"r") as f:
            config = json.load(f)
            logging.config.dictConfig(config)
    else:
        logging.basicConfig(level = default_level)

def func():
    logging.info("start func")

    logging.info("exec func")

    logging.info("end func")

if __name__ == "__main__":
    setup_logging(default_path = "logging.json")
    func()
```

除此之外，logging.dictConfig还支持yaml文件来配置。

参考文件：

[Python标准模块--logging](https://www.cnblogs.com/zhbzz2007/p/5943685.html)

[python中logging模块的一些简单用法](https://www.cnblogs.com/CJOKER/p/8295272.html)

.
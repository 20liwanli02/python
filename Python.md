## Python-4366













### 一、环境搭建与基础语法

#### 1、环境搭建与脚本结构

##### （4）变量与关键字

#### 2、数据类型

##### 附录





### 二、三大主流数据类型的操作

##### 1、字符串的常用方法

###### （3）字符串函数及其操作

**capitalize()**

功能：首字母变为大写，其他部分变为小写

用法：

newstr = string.capitalize

注意：

产生新的字符串，然后赋值给变量



**lower()与casefold()**

功能：全部变为小写

用法：

newstr = string.casefold()

newstr = string.lower()

注意：

产生新的字符串，然后赋值给变量

只对字符串的字母有效



**upper()**

功能：全部变为大写

用法：

newstr = string.upper()

注意：

产生新的字符串，然后赋值给变量

只对字符串的字母有效



**swapcase()**

**zfill()**

**count()**

**startwith()/endswith**

**find()/index()**

**strip()/lstrip()/rstrip()**



###### （4）字符串编码格式

#coding: utf-8



##### （5）字符串格式化



##### （6）字符串的特殊字符



##### 2、列表的常用操作



##### 3、字典的常用方法





### 三、流程控制与函数

##### 1、集合的常用方法

##### 2、数据类型转换

##### 3、流程控制

##### 4、函数的定义与使用



















### 四、面向对象与异常处理

##### 1、面向对象编程

###### （2）初识面对对象

解释：使用对象的属性或者方法称为面向对象编程

类的定义与调用：

class Name(object):

​	attr = something

​	def func(self):#self写在第一个参数位

​		do

self：

代表实例化变量本身，调用类属性。self.name

定义一个变量，self.name = name

类中添加方法一定要添加self，self代表类的实例化对象，方法添加self等于将方法注册到类中，方法才能在其他的地方被调用

类的构造函数：实例化对象的同时传递参数

```python
def __init__(self,a,b):
    self.a = a
    self.b = b
```

对象的生命周期：

实例化__init__()对象生命的开始

编译器运行结束删除__del__() 对象

左右都有两下划线的函数，都是类的内置函数

###### （3）类的私有函数与私有变量

解释：无法被实例化对象调用类中定义的方法与属性，只能在类中调用

定义：在变量或函数前添加两个下横线

```python
class Person(object):
    def __init__(self,name):
		self.name = name
        self.__age = 33
    def dump(self):
        print(self.name,self.__age)
    def __cry(self):
        return 'I want cry'
```

封装：将不对外部使用的私有的方法或者属性通过可以对外使用的方法而对外使用

```python
class Person(object):
    def __hello(self,data):
		print(data)
    def helloword(self):
        self.__hello('Hello World!')
```

###### （4）装饰器与类的装饰器

解释：一种函数，接受处理函数，返回函数

定义：

```python
def out(func_args):#外围函数
    def inter(*args,**kwargs):#内嵌函数
        return func_args(*args,**kwargs)
    return inter#外围函数返回内嵌函数
```

使用：

```python
#First
def a(func):
	def b(*args,**kwargs):
        return func(*args,**kwargs)
    return b

def c(name):
    print(name)

a(c)('Hello World!')#Hello World!

----------------------------------------------------------------
#Second
@a
def c(name):
    print(name)

c('Hello World!')

----------------------------------------------------------------
def check_str(func):
    def inner(*args,**kwargs):
        result = func(*args,**kwargs)
        if result == 'OK':
            return 'result is %s' % result
        else:
            return 'result is failed'
    return inner

@check_str
def test(data):
    return data

result = test('NO')
```

常用的类装饰器

classmethod：该类函数可以不实例化直接被调用

参数：

cls：替代self，代表当前操作类

用法：

```python
class Test(object):
	@classmethod
	def add(cls,a,b):
        return a+b
Test.add(1,2)
----------------------------------------------------------------
class Test(object):
    def __init__(self):
        self.a = a
        
    def run(self):
        print('run')
        #self.dump()普通self函数可以调用classmethod装饰的函数
        
    @classmethod
    def dump(cls):
        print('dump')
        #cls.run()无法在classmethod装饰的函数中用cls调用普通self的函数
        
t = Test('a')
t.run()#run
Test.run()#error,在实例化的过程中python会将self传入需要self函数的括弧中，但现在没有实例化，所以因为没有传递self而出错
Test.dump()#dump,无需传递self
t.dump()#dump
```



staticmethod

解释：该函数不经过实例化可被直接调用，不用传递self和cls，且无法在该函数内调用其他函数或类变量

使用：

```python
class Test(object):
    def __init__(self):
        self.a = a
        
    def run(self):
        print('run')
        #self.sleep()普通self函数可以调用staticmethod装饰的函数

    @classmethod
    def dump(cls):
        print('dump')

    @staticmethod
	def sleep():
        print('I want sleep')

t = Test('a')
Test.sleep()#'I want sleep'
t.sleep()#'I want sleep'
```



property

解释：使函数的执行免去括弧，类似于调用属性

```python
class Test(object):
    def __init__(self):
        self.name = name
	
    @property
	def call_name(self):
        return 'hello {}'.format(self.name)

test = Test('May')
result = test.call_name
print(result)#hello May
----------------------------------------------------------------
class Test(object):
    def __init__(self):
        self.__name = name
	
    @property
	def name(self):
        return self.__name
    
    @name.setter
    def name(self,value):
        self.__name = value

test = Test('May')
test.name = 'Jun'
print(test.name)#Jun
```



###### （5）类的继承与多态

继承：子类拥有父类的所有属性与方法

多态：拥有同一个函数，却执行出不同的结果

super：使子类完全使用父类的方法，不仅仅使用父类方法的名字

使用：

```python
class Parent(object):
    def __init__(self):
        print('Hello')

class Child(Parent):
    def __init__(self):
        print('World')
        super(Child,self).__init__()#用的是父类的方法
----------------------------------------------------------------
class Parent(object):
    def __init__(self,p):
        print('Hello %s' % p)

class Child(Parent):
    def __init__(self,c,p):
        print('World %s' % c)
        super().__init__(p)#用的是父类的方法

c = Child(c='Child',p='Parent')#
```



###### （6）类的多重继承

解释：继承多个父类

使用：

```python
class Child(Parent1,Parent2)
```



###### （7）类的高级函数

__str__()：如果定义的此函数，print实例化对象会返回str的格式过的信息；没有定义此函数，print实例化对象输出内存地址



__getattr__()：调用的属性或者方法不存在时，返回方法定义的信息

```python
class Test(object):
    def __getattr__(self,key):
        print('key is error')

t = Test()
print(t.a)#'key is error' None
```



__setattr__()：拦截当前不存在的属性或者值

```python
class Test(object):
	def __setattr__(self,key,value):
        self.__dict__[key] = value
        print(self.__dict__)

t = Test()
t.name = 'May'#
```



__call__：将一个实例化对象当函数使用

```python
class Test(object):
	def __call__(self,a):
        pritn(a)

t = Test()
t('May')#May
```



混合实例：

```python
class Test(object):
    def __init__(self,attr):
        self.__attr = attr

	def __call__(self):
		print('key is {}'.format(self.__attr))

	def __getattr__(self,key):
		if self.__attr:
            key = '{}.{}'.format(self.__attr,key)
        else:
			key = key
        return Test(key)

t = Test()
t.a.b.c()
#a
#a.b
#a.b.c
#key is a.b.c
```





##### 2、异常处理机制

###### （1）初识异常

try中出错不会继续执行try的语句

except捕获一个异常后不会再捕获异常



###### （2）内置异常函数

Exception

ZeroDivisionError

AttrbuteError

IOError

IndexError

KeyError

NameError

SyntaxError

SystemError

ValueError



###### （3）finally

无论是否发生异常都将执行finally，包括进行return依然执行

try至少要伴随except或者finally中的一个



###### （4）自定义异常与主动抛出

raise：将信息以报错的形式进行抛出

用法：

raise 异常类型(message)

参数message，错误信息

无返回值

```python
def test(number):
    if number == 100:
        raise ValueError('number 不可以是100')
    return number

result = test(50)
print(result)#50
result = test(100)
print(result)#ValueError:number 不可以是100
----------------------------------------------------------------
def test2(number):
    try:
		return test(number)
	except ValueError as e:
        return e

result = test2(100)
print(result)#ValueError:number 不可以是100
----------------------------------------------------------------def test3():
	raise '错误了'

test3()#错误，raise后面要带有异常类型
----------------------------------------------------------------
def test4(name):
	if name == 'May':
        raise Exception('Error')

test4('May')#Exception:Error
```



自定义异常类

继承Exception

拥有传递错误信息的初始化函数、

```python
class NumberLimitError(Exception):
    def __init__(self,message):
        self.message = message


class NameLimitError(Exception):
    def __init__(self,message):
        self.message = message

def test(name):
	if name == 'May':
		raise NameLimitError('name is error')
	return name

def test1(number):
    if number > 100:
        raise NumberLimitError('number is bigger than 100')
	return numebr

try:
    test('May')
except NameLimitError as e:
    print(e)#name is error

try:
    test1(150)
except NumberLimitError as e:
    print(e)#number si bigger than 100
```



###### （5）断言

判断表达式，表达式位False触发异常

asserts用法：

assert expression,message

```python
assert len(kwargs) == 3,'参数必须是3个'
```



###### （6）调试方法









### 五、模块与文件操作

##### 1、模块和包

###### （1）包与模块的定义与导入

创建包：内部带一个__init__.py文件



包导入：

import package

```python
import animal
```



模块导入：

from package import module

```python
from animal.cat import action#animal包下的cat包下的actionpy文件
from animal.cat.action import run#animal包下的cat包下的actionpy文件里的run方法
from animal.cat.action import run as e#run方法更名为e，防止重名
```



###### （3）第三方包

pip与easy_install安装第三包

eg.

```python
pip install ipython == '版本号'
```



###### （4）datetime与time

时间对象：2023-10-28 20:10:51.609983

时间戳：1698495673.3965814

格式化的时间戳：2023-10-28 20:10:51





datetime：获取当前时间，时间间隔，时间对象转时间字符串，字符串转时间类型

datetime获取当前时间：

```python
#第一种
from datetime import datetime
datetime.now()
#第二种
import daatime
datetime.datetime.noew()
```



datetime获取时间间隔：

```python
from datetime import datetime
from datetime import timedelta
before_one_day = timedelta(days=1)
yestoday = datetime.now() - before_one_day
print(yestoday)
```



datetime时间对象转字符串

```python
from datetime import datetime
date = datetime.now()
str_date = date.strftime('%Y-%m-%d %H:%M:%S')
print(str_date)#'2020-03-17 15:19:27'
```



datetime时间字符串转时间对象

```python
from datetime import datetime
str_date = '2020-03-17 15:19:27'
date_obj = datetime.strptime(str_date,'%Y-%m-%d %H:%M:%S‘)
print(date_obj)#2020-03-17 15:19:27
```



datetime生成时间戳

```python
impotr datetime
now = datetime.datetime.now()
datetime.datetime.timestamp(now)

#不同与time，time是生成时间戳，而datetime是转成时间戳，所以括弧中必须传入参数，如上面的now
```



datetime时间戳转时间对象

```python
import datetime
datetime.datetime.fromtimestamp(timestamp)
```



time:

时间戳：从1970年1月1日00时00分00秒至今的总毫秒数



生成时间戳

```python
import time
time.time()
```



把时间戳转为对应的时间对象

```python
import time
time.localtime(timestamp)#timestamp时间戳可以不传递，不传递返回当前最新时间戳、的时间对象
```



暂停函数sleep：

```python
import time
time.sleep(1)
```



time的strftime时间对象转字符串

```python
import time
time.strftime(format,t)

format：格式化规范
t：time.localtime对应的时间类型
```



time的strptime时间字符串转时间对象

```python
import time
time.strptime('2020-12-12','%Y-%m-%d')

time：符合时间格式的字符串
format：确保与time_srt一致格式化标准
```



###### （5）os与sys模块

os模块：

相对路径，相对到当前目录下



getcwd()：返回当前所在路径

```python
import os
path = os.getcwd()
print(path)#C:\Test,打印了当前文件的路径，在c盘的Test下
```



listdr()：返回指定路径下的所有的文件或文件夹

```python
import os
path = os.getcwd()

new_path = '%s/test1' % path
os.makedirs(new_path)

data = os.listdir(path)
print(data)#打印出所有的文件
```



makedirs()：创建多级文件夹

多层创建，没有的文件夹会自动创建

没指定路径，默认在当前项目下创建，相对路劲

```python
import os
path = os.getcwd()

new_path = '%s/test1' % path
os.makedirs(new_path)#在C盘的Test文件夹下创建一个test1文件
```



removedirs()：删除多级文件

```python
os.removedirs('test2/abc')#相对路径，删除项目下的test2文件和它下面的abc文件
```



rename()：修改文件名字

```python
os.rename('test3','test3_new')#test3更名为test3_new
```



rmdir()：只能删除空的文件夹

```python
os.rmdir('test3')
```





os中的path模块：

exists：文件或路径是否存在，返回bool类型

isdir：是否是路径，返回bool类型

isabs：是否是绝对路径，返回bool类型

isfile：是否是文件，返回bool类型，文件名既是文件名又是路径，所以参数是传入一个路径

join：路径字符串合并，字符串

split：以最后一层路径为基准切割，把最后一层分出来，元组





sys模块：

modules：python启动时加载的模块

path()：返回当前python的环境路径

exit()：退出程序

getdefaultencoding()：获取系统编码，如utf-8

platform()：获取当前系统平台

version：获取python、版本

argv：程序外部获取参数，返回一个列表







##### 2、文件操作

###### （1）文件的读写

文件的创建与写入

```python
import os

#def create_package(path):
   # if os.path.exists(path):
    #    raise Exception('%s 已经存在不可创建' % path)
	#os.makedirs(path)
   # init_path = os.path.join(path,'__init__.py')
    #f = open(init_path,'w')
    #f.write('# coding:utf-8\n')
    #f.close()

class Open(object):
    def __init__(self,path,mode='w',is_return=True):
		self.path = path
        self.mode = mode
        self.is_return = is_return

	def write(self,message):
        f = open(self.path,mode=self.mode)
        if self.is_return:
            message = '%s\n' % message
		f.write(message)
        f.close()


if __name__ == '__main__':
    current_path = os.getcwd()
    #path = os.path.join(current_path,'test1')
    #create_package(path)
    open_path = os.path.join(current_path,'b.txt')
    o = Open(open_path)
    o.write('Hello World!')
```



文件的读操作

```python

```















##### 3、常用函数与高阶函数

###### （1）加密工具

byte：比特类型，encode编码字符串为比特（byte）类型，decode解码比特为字符串类型



hashlib参数byte(utf-8)，返回哈希对象,hexdigest()：以十六进制进行输出:

md5

sh1

sh256

sha512



eg.

```python
#再次加密进行验证是否正确可行

import time

base_sign = 'muke'

def custom():
	a_timestamp = int(time.time())
	_toke = '%s%s' % (base_sign, a_timestamp)
    hashobj = hashlib.sha1(_token.encode('utf-8'))
    a_token = hashobj.hexigest()
    return a_token, a_timestamp

def b_service_check(token, timestamp):
    _token = '%s%s' % (base_sign, timestamp)#让被加密的内容保持格式相同
    b_token = hashlib.sha1(_token.ecode('utf-8')).hexigest()
    if token == b_token:
        retutn True
    else:
        return False


if __name__ == '__main__':
    need_help_token, timestamp = custom()#一个参数接受返回元组
    result = b_service_check(need_help_token, timestamp)
    if result == True:
        print('Yes')
    else:
        print('No')
```



base64，参数为bite，结果也都为比特：

encodebytes

decodebytes



eg.

```python
replace_one = '%'
replace_two = '$'

def encode(data):
	if isinstance(data, str):
        data = data.encode('utf-8')
	else isinstande(data, bytes):
        data = data
    else:
        raise TypeError('data need bytes or str')

	_data = base64.encodebytes(data).decode('utf-8')
	_data = _data.replace('a', replace_one).replace('2', replace_two)
    return _data

def decode(data):
    if not isinstance(data, bytes):
		raise TypeError('data is not bytes')

	replace_one_b = replace_one.encode('utf-8')
	replace_two_b = replace_two.encode('utf-8')
	data = data.replace(replace_one_b, b'a').replace(replace_two_b, b'2')

	return base64.decodebytes(data).decode('utf-8')

if __name__ == '__main__':
    result = encode('hello')
	new_result = decode(result.encode('utf-8'))
```





###### （2）日志模块

等级：

debug

info

warnning

error

critical



使用：

level：日志记录的最低等级

format：日志的格式

​	%(levelname)s：日志级别

​	%(pathname)s：执行程序的路径，脚本的路径

​	%(filename)s：执行程序的名称，脚本的名字

​	%(lineno)d：日志的当前行号

​	%(asctime)s：打印日志的时间

​	%(message)s：日志的信息

​	常用格式：

​		format=‘%(asctime)s%(filename)s[line:%(lineno)d]%(levelname)s%						(message)s’

filename：日志的文件存储位置

filemode：日志的输入模式



logging.error：函数

logging.ERROR：查看ERROR的日志等级



eg.

```python
# coding:utf-8

import logging
import os

def init_log(path):
    if os.path.exists(path):
        mode = 'a'
    else:
        mode = 'w'
    logging.basicConfig(
        level=logging.INFO,
        format='%(asctime)s %(filename)s %(lineno)d %(levelname)s %(message)s',
        filename=path,
        filemode=mode
    )
    return logging

current_path = os.getcwd()
path = os.path.join(current_path, 'back.log')
log = init_log(path)

log.info('这是第一个记录的日志信息')
log.warning('这是一个警告')
log.error('这是一个重大的错误信息')
log.debug('这是一个debug')
```





###### （3）虚拟环境

解释：拥有多个python环境时，使用虚拟环境使各个python独立运行



工具：
Virtualenv：电脑有某版本的python，才能创建对应某python版本的虚拟环境

pyenv：不局限电脑存在的python



pip install virtualenv

命令行下使用

选择目录

选择python版本：virtualenv -p python3.x env3.x

进入：(source) ./env3.x/bin/activate

退出：deactivate





###### （4）常用函数的集合

内置函数总结：
abs(Number)：返回数字绝对值

all(List)：判断列表的内容是否全是True，全为True才为True

help(object)：打印对象的用法

enumerate(iterable)：迭代时记录索引

input(str)：命令行输入信息



isinstance(Object,type)：判断对象是否时某种类型

type(Object)：判断对象的类型

vars(instance)：以字典的形式返回实例化对象的信息

dir(Object)：返回对象中可用的方法和属性

hasattr(Object,key)：判断对象中是否有某个属性



setattr(Object,key,value)：为实例化对象添加属性与值

getattr(Object,key)：通过对象获取属性

any(Iterable)：判断内容是否为true值，一个为True就返回True



eg.

```python
# coding:utf-8

# food = input('你想吃什么呢：')
# print(food)
#
# help(input)

class Test(object):
    a = 1
    b = 2

    def __init__(self):
        self.a = self.a
        self.b = self.b


test = Test()
print(test.a)
result = vars(test)
print(result)

print(hasattr(test, 'a'))
print(hasattr(list, 'appends'))#是python内置的列表类

setattr(test, 'c', 3)
print(test.c)
print(vars(test))

# setattr(list, 'c', 1)#错误
if hasattr(list, 'appends'):
    print(getattr(list, 'appends'))
else:
    print('不能存在')

a = ['', None, True, 0]
print(any(a))
# all - > and
# any - > or
```





###### （5）random模块

 random.random()：随机返回0~1之间的浮点数

random.uniform()：产生一个a，b区间的随机浮点数

random.randint()：产生一个a，b区间的随机整数

random.choice()：返回对象中的一个随机元素

random.sample()：随机返回对象中指定数量的元素

random.randrange()：获取区间内的随机数



eg.

```python
# coding:utf-8

import random

gifts = ['iphone', 'ipad', 'car', 'tv']

def chioce_gifts():
    gift = random.choice(gifts)
    print('你得到了%s' % gift)


def chioce_gift_new():
    count = random.randrange(0, 100, 1)
    if 0 <= count <= 50:
        print('你中了一个iphone')
    elif 50 < count <= 70:
        print('你中了一个ipad')
    elif 70 < count < 90:
        print('你中了一个tv电视')
    elif count >= 90:
        print('恭喜你中了一辆小汽车')

if __name__ == '__main__':
    chioce_gift_new()
```





###### （6）python高阶函数

迭代器：

解释：写入时不用一次性的写入，边写边写入

生成迭代器对象：
iterator = iter(iterable)

iterable：可迭代的数据类型

使用next：

next(iterator)

常用方法生成迭代器：

yield：每个for出来的对象都生成一个迭代器

for循环一行生成迭代器

获取：

next()

for循环





eg.

```python
# coding:utf-8

iter_obj = iter((1, 2, 3))

def _next(iter_obj):
    try:
        return next(iter_obj)
    except StopIteration:
        return None

# print(_next(iter_obj))
# print(_next(iter_obj))
# print(_next(iter_obj))
# print(_next(iter_obj))

#
def make_iter():
    for i in range(10):
        yield i

iter_obj = make_iter()

for i in iter_obj:
    print(i)
print('----')
for i in iter_obj:
    print(i)


#
iter_obj = (i for i in range(10))

for i in iter_obj:
    print(i)
print('=====')
for i in iter_obj:
    print(i)
```





filter

filter(func,list)

由func对list进行帅选



map

map(func,list)

由func对list进行帅选，成立True，不成立False



reduce

from functools import reduce

reduce(func,list)

func数据累加



eg.

```python
# coding:utf-8

from functools import reduce

frunts = ['apple', 'banana', 'orange']

result = filter(lambda x: 'e' in x, frunts)
#result是filter对象要转成list列表查看
print(list(result))
print(frunts)


def filter_func(item):
    if 'e' in item:
        return True
print('-------')
filter_result = filter(filter_func, frunts)
print(list(filter_result))


map_result = map(filter_func, frunts)  # > all
print(list(map_result))


reduce_result = reduce(lambda x, y: x * y, [1, 1, 2, 4, 4])
print(reduce_result)#不用list

reduce_result_str = reduce(lambda x, y: x * y, frunts)
print(reduce_result_str)
```











### 六、多线程编程、正则表达式与综合项目实战

##### 1、进程与线程编程

###### （2）进程与多进程

###### （4 ）创建多进程multiprocessing：

process(target,args)：返回进程对象，创建一个进程

start()：执行进程

join()：阻塞进程

kill()：杀死进程

is_alive() ：判断进程是否存活



eg.

```python
# coding:utf-8

import time
import os
import multiprocessing

def work_a():
    for i in range(10):
        print(i, 'a', os.getpid())#返回当前执行的进程的号码         
        time.sleep(1)

def work_b():
    for i in range(10):
        print(i, 'b', os.getpid())
        time.sleep(1)


if __name__ == '__main__':
    start = time.time() # 主进程1
    a_p = multiprocessing.Process(target=work_a)  # 子进程1
    # a_p.start()  # 子进程1执行
    # a_p.join()
    b_p = multiprocessing.Process(target=work_b)  # 子进程2
    # b_p.start()  # 子进程2执行

    for p in (a_p, b_p):
        p.start()

    for p in (a_p, b_p):#
        p.join()

    for p in (a_p, b_p):
        print(p.is_alive())

    print('时间消耗是：', time.time() - start)  # 主进程代码2
    print('parent pid is %s' % os.getpid())  # 主进程代码3行
```





###### （5）进程池和进程锁

进程池：避免出现过多的进程，固定进程数量，池里的进程的开与关由进程池决定

创建进程池：

pool()：返回进程池对象

apply_async(function，args)：任务加入进程池

close()：关闭进程池

join()：等待进程池让你无结束



eg.

```python
# coding:utf-8

import os
import time
import multiprocessing


def work(count, lock):
    lock.acquire()#上锁
    print(count, os.getpid())
    time.sleep(5)
    lock.release()#开锁
    return 'result is %s, pid is %s' % (count, os.getpid())


if __name__ == '__main__':
    pool = multiprocessing.Pool(5)
    manger = multiprocessing.Manager()
    lock = manger.Lock()
    results = []
    for i in range(20):
        result = pool.apply_async(func=work, args=(i, lock))#异步可以由返回值
        # results.append(result)

    # for res in results:
    #     print(res.get())

    pool.close()#陪着join一起使用
    pool.join()#阻塞主进程
```





###### （6）进程之间的通信

queue()：创建队列

put()：信息放入队列

get()：获取队列信息（str）

target：任务动作



eg.

```python
# coding:utf-8

import time
import json
import multiprocessing

class Work(object):
    def __init__(self, q):
        self.q = q

    def send(self, message):
        if not isinstance(message, str):
            message = json.dumps(message)
        self.q.put(message)

    def send_all(self):
        for i in range(20):
            self.q.put(i)
            time.sleep(1)

    def receive(self):
        while 1:
            result = self.q.get()
            #利用异常处理非常巧妙的避免程序停止且同时进行选择
            try:
                res = json.loads(result)
            except:
                res = result
            print('recv is %s' % res)


if __name__ == '__main__':
    q = multiprocessing.Queue()
    work = Work(q)
    send = multiprocessing.Process(target=work.send, args=({'name': '小慕'},))
    recv = multiprocessing.Process(target=work.receive)
    send_all_p = multiprocessing.Process(target=work.send_all)

    send_all_p.start()
    send.start()
    recv.start()

    send_all_p.join()
    recv.terminate()#结束进程recv
```





###### （7）线程的创建

Thread()

start()：启动线程

join()：阻塞主线程

getName()：获取线程的名字

setName()：设置线程的名字

is_alive()：判断线程是否还活着

setDaemon()：守护线程，主线程执行完允许子线程执行，而不是结束程序的执行



eg.

```python
# coding:utf-8

import time
import random
import threading

lists = ['python', 'django', 'tornado',
         'flask', 'bs5', 'requests', 'uvloop'
]

new_lists = []


def work():
    if len(lists) == 0:
        return
    data = random.choice(lists)
    lists.remove(data)
    new_data = '%s_new' % data
    new_lists.append(new_data)
    time.sleep(1)


if __name__ == '__main__':
    start = time.time()
    print('old list len:', len(lists))
    t_list = []
    for i in range(len(lists)):
        t = threading.Thread(target=work)
        t_list.append(t)
        t.start()

    for t in t_list:
        t.join()

    print('old list:', lists)
    print('new list:', new_lists)
    print('new_list len', len(new_lists))
    print('time is %s' % (time.time() - start))
```





###### （8）线程池

创建线程池：
from concurrent.futures import ThreadPoolExecutor

t = ThreadPoolExecutor(2)



submit()：加入任务到线程池

done()：线程池中的某个线程是否完成了任务

result()：获取当前线程执行任务的结果





eg.

```python
#多个线程依然是在一个进程下完成的

# coding:utf-8

import time
import os
import threading

from concurrent.futures import ThreadPoolExecutor


lock = threading.Lock()#线程锁


def work(i):
    # lock.acquire()
    print(i, os.getpid())
    time.sleep(1)
    # lock.release()
    return 'result %s' % i


if __name__ == '__main__':
    print(os.getpid())
    t = ThreadPoolExecutor(2)
    result = []
    for i in range(20):
        t_result = t.submit(work, (i, ))#没有target，多个并发，但不是一次性返回两个值
        result.append(t_result)

    for res in result:
        print(res.result())
```





###### （9）全局锁GIL

别人家的线程与我家的线程

解决：

使用pypy解释器

多进程＋多线程





###### （10）异步

asyncio

 async：定义异步

await：执行异步

gather：异步函数批量执行

run：执行主异步函数



gevent

spawn：创建协程对象

joinall：批量处理协程对象



```python
# coding:utf-8

import os
import time
import random
import asyncio
import gevent


def gevent_a():
    for i in range(5):
        print(i, 'a gevent', os.getpid())
        gevent.sleep(random.random() * 2)
    return 'gevent a result'


def gevent_b():
    for i in range(5):
        print(i, 'b gevent', os.getpid())
        gevent.sleep(random.random() * 2)
    return 'b gevent result'



async def a():
    for i in range(10):
        print(i, 'a', os.getpid())
        await asyncio.sleep(random.random() * 2)
    return 'a function'


async def b():
    for i in range(10):
        print(i, 'b', os.getpid())
        await asyncio.sleep(random.random() * 2)
    return 'b function'


async def main():
    result = await asyncio.gather(
        a(),
        b()
    )
    print(result[0], result[1])


if __name__ == '__main__':
    start = time.time()
    # asyncio.run(main())

    g_a = gevent.spawn(gevent_a)
    g_a.run()
    g_b = gevent.spawn(gevent_b)
    gevent_list = [g_a, g_b]
    result = gevent.joinall(gevent_list)
    print(result[0].value)#gevent a result
    print(result[1].value)#b gevent result
    print(time.time() - start)
    print('parent is %s' % os.getpid())
```







##### 2、正则表达式

##### 3、项目抽奖系统













### 附录
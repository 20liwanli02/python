### Django3 Web

#### 一、django建站基础

###### 网站的开发流程：



###### MTV

- 模型：数据存取层，处理与数据相关的所有事务，和数据库息息相关；
- 模板：表现层，处理与表现相关的决定，存放html框架；
- 视图：显示的页面；



###### 初始化命令：

MyDjango项目：django-admin startproject MyDjango

index应用：python manage.py startapp index

运行：python manage.py runserver [8001](默认为8000)



###### 编写Hello World!!：

pycharm创建项目Mydjango

terminal创建应用index

![Terminal](../AppData/Roaming/Typora/typora-user-images/image-20231014150404736.png)

![result](../AppData/Roaming/Typora/typora-user-images/image-20231014150524119.png)





##### 附录







#### 二、django配置信息

###### 基本配置信息：

CSRF：与POST有关，用于表单提交，直接关掉可以进行post请求；

ALLOWED_HOSTS：设置可访问的域名；

DEBUG：是否开启调试模式；

INSTALLED_APPS：有哪些APP应用



###### 资源文件配置

静态资源：

```python
#http://127.0.0.1:8000/static/...
STATIC_URL = '/static/'
#index中的static默认可以访问到不用添加,但根目录的静态文件和index新建的M有static文件不会默认访问到，所以要添加
STATICFILES_DIRS = [
    # 设置根目录的静态资源文件夹static
    BASE_DIR / 'static',
    # 设置App（index）的静态资源文件夹Mystatic
    BASE_DIR / 'index/Mystatic',
]
#DEBUG=False时设置STATIC_ROOT,STATIC_ROOT会把所有的静态资源搜集且保存到STATIC_ROOT设置的文件夹里
```

媒体资源：

```python
# 设置媒体路由地址信息
MEDIA_URL = '/media/'
# 添加根目录的media文件夹路径
MEDIA_ROOT = BASE_DIR / 'media'
```



###### 模板配置

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        #配置了两个模板，根目录与应用index，一般情况下根目录模板存放各应用的通用模板
        'DIRS': [BASE_DIR / 'templates',
                 BASE_DIR / 'index/templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```



###### 数据库配置

```python
#django默认自带的sqlite3数据库
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

```python
#链接mysql，先安装mysqlclient
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER': 'root',
        'PASSWORD': '1234',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
#使用pymysql还要在_init_添加
import pymysql
pymysql.install_as_MySQLdb()
#gjango链接sql8.0可能因为加密链接失败
```

```python
# 多数据库连接
DATABASES = {
# 第一个数据库
'default': {
         'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER':'root',
         'PASSWORD':'1234',
         'HOST':'127.0.0.1',
         'PORT':'3306',
     },
# 第二个数据库
'MyDjango': {
         'ENGINE': 'django.db.backends.mysql',
         'NAME': 'mydjango_db',
         'USER':'root',
         'PASSWORD':'1234',
         'HOST':'127.0.0.1',
         'PORT':'3306',
     },
# 第三个数据库
'MySqlite3': {
         'ENGINE': 'django.db.backends.sqlite3',
         'NAME': BASE_DIR / 'sqlite3',
     },
}
```

```python
#配置文件动态链接数据库，使切换链接的数据库时无需重启django服务以及修改setting.py，使用my.cnf确保路径无中文
#根目录下创建my.cnf文件，编写如下：
[client]
database=django_db
user=root
password=123456
host=127.0.0.1
port=3306
#第二步：
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {'read_default_file': str(BASE_DIR / 'my.cnf')},
    }
}
```

```python
#SSH隧道远程链接MySQL
```



###### 中间件

```python
#中间件可以处理请求信息，然后返回给用户
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    #使gjango内置的功能支持中文显示
    'django.middleware.locale.LocaleMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```



##### 附录







####  三、路由

###### 项目的基本路径流程

```python
#项目的基本路径流程
#项目启动首先寻找setting.py中的ROOT_URLCONF = 'MyDjango.urls'
#MyDjango的urls.py
urlpatterns = [
    # 指向内置admin系统的路由文件sites.py
    path('admin/', admin.site.urls),
    # 指向index的路由文件urls.py
    #http://127.0.0.1:8000/
    path('', include('index.urls')),
]
#index的urls.py
urlpatterns = [
    #http://127.0.0.1:8000/
    #views.index代表视图函数index
    path('', views.index)
]
#index的views.py
def index(request):
    value = 'This is test!'
    print(value)
    #render返回templates中的html模板文件index.html
    return render(request, 'index.html')
```



###### **拒绝路由重复编写**

- **字符类型**：匹配任何非空字符串，但不含斜杠。<u>如果没有指定类型，就默认使用该类型</u>。
- **整型**：匹配0和正整数。
- **slug**：可理解为注释、后缀或附属等概念，常作为路由的解释性字符。可匹配任何ASCII字符以及连接符和下画线，能使路由更加清晰易懂。比如网页的标题是“13岁的孩子”，其路由地址可以设置为“13-sui-de-hai-zi”。
- **uuid**：匹配一个uuid格式的对象。为了防止冲突，规定必须使用“-”并且所有字母必须小写，例如075194d3-6885-417e-a8a8-6c931e272f00。

```python
#MyDjango的urls.py
urlpatterns = [
指向内置admin系统的路由文件sites.py
    path('admin/', admin.site.urls),
    # 指向index的路由文件urls.py
    path('', include('index.urls')),
]
#index的urls.py
urlpatterns = [
    # 添加带有字符类型、整型和slug的URL
    #http://127.0.0.1:8000/2018/05/01.html
    #路由地址的末端设置了“.html”，这是一种伪静态URL技术，可将网址设置为静态网址，用于SEO搜索引擎的爬取，如百度、谷歌等。此外，在末端设置“.html”是为变量day设置终止符，假如末端没有设置“.html”，在浏览器上输入无限长的字符串，路由也能正常访问
    path('<year>/<int:month>/<slug:day>', views.myvariable),
    # 添加路由地址信息外的变量month
    #http://127.0.0.1:8000/
    path('', views.index, {'month': '2019/10/10'})py
]
#index的views.py
def myvariable(request, year, month, day):
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))

def index(request, month):
    return HttpResponse('这是路由地址信息之外的变量：' + month)
```



###### 正则表达式编写路由

```python
#MyDjango的urls.py
urlpatterns = [
指向内置admin系统的路由文件sites.py
    path('admin/', admin.site.urls),
    # 指向index的路由文件urls.py
    path('', include('index.urls')),
]
#index的urls.py
urlpatterns = [
re_path('(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2}).html', views.mydate)
]
#index的views.py
def mydate(request, year, month, day):
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))
```



###### 命名空间namespace和路由命名name

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向内置admin系统的路由文件sites.py
    path('admin/', admin.site.urls),
    # 指向index的路由文件urls.py
    #使用namespace，arg为元组
    #eg：include(('index.urls', 'index')一定要有第二个参数index，第二个参数可以不为index
    #namespace是include函数的参数
    path('', include(('index.urls', 'index'), namespace='index')),
    # 指向user的路由文件urls.py
    path('user/', include(('user.urls', 'user'), namespace='user'))
]
#index的urls.py
urlpatterns = [
    #name是path函数的参数
    #name给对应的视图函数起名字
    re_path('(?P<year>[0-9]{4}).html', views.mydate, name='mydate'),
    path('', views.index, name='index')
]
#index的views.py
def mydate(request, year):
    return HttpResponse(str(year))

def index(request):
    return render(request, 'index.html')
#templates的index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
    {# 添加错误代码static#}
    {#{% static %}#}
    {# 添加正确代码，但不出现在网页 #}
    {# <div>Hi,{{ value }}</div> #}
    <span>Hello World!!</span>
</body>
</html>
```



###### 模板中使用路由

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向内置admin系统的路由文件sites.py
    path('admin/', admin.site.urls),
    # 指向index的路由文件urls.py
    # path('', include('index.urls')),
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 添加带有字符类型、整型和slug的路由
    path('<year>/<int:month>/<slug:day>', views.mydate, name='mydate'),
    # 定义首页的路由
    path('', views.index, name='index')
]
#index的views.py
def mydate(request, year, month, day):
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))

def index(request):
    return render(request, 'index.html')
#templates的index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
    <span>Hello World!!</span>
    <br>
    #
    {# <a href="{% url 'mydate' '2019' '01' '10' %}">查看日期</a>#}
    <a href="{% url 'index:mydate' '2019' '01' '10' %}">查看日期</a>
</body>
</html>
```



###### 反向解析reverse与resolve

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 添加带有字符类型、整型和slug的路由
    path('<year>/<int:month>/<slug:day>', views.mydate, name='mydate'),
    # 定义首页的路由
    path('', views.index, name='index')
]
#index的views.py
def mydate(request, year, month, day):
    args = ['2019', '12', '12']
    #resolve通过路由地址生成路由对象信息
    result = resolve(reverse('index:mydate', args=args))
    #分析代入args的变量与args元素的对应关系
    print('kwargs：', result.kwargs)
    #path的name
    print('url_name：', result.url_name)
    #include的namespace
    print('namespace：', result.namespace)
    #namespace:name，哈哈、因为在视图函数里面url的语法一般为namespace:name
    print('view_name：', result.view_name)
    #应用名
    print('app_name：', result.app_name)
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))

def index(request):
    kwargs = {'year': 2010, 'month': 2, 'day': 10}
    args = ['2019', '12', '12']
    #reverse生成路由地址，viewname一般以路由命名name生成
    #arg以列表的方式出传递路由地址变量
    #reverse('index:mydate', args=args)返回<year>/<int:month>/<slug:day>再带入args，前面加'/',/2019/12/12
    print(reverse('index:mydate', args=args))
    #kwargs以字典的方式出传递路由地址变量
    print(reverse('index:mydate', kwargs=kwargs))
    return HttpResponse(reverse('index:mydate', args=args))
#templates的index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
    <span>Hello World!!</span>
</body>
</html>
```



###### 路由重定向

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 添加带有字符类型、整型和slug的路由
    path('<year>/<int:month>/<slug:day>', views.mydate, name='mydate'),
    # 定义首页的路由
    path('', views.index, name='index'),
    # 设置路由跳转
    path('turnTo', RedirectView.as_view(url='/'), name='turnTo'),
    # path('turnTo', RedirectView.as_view(pattern_name='index:index'), name='turnTo'),
]
#index的views.py
def mydate(request, year, month, day):
    return HttpResponse(str(year) + '/' + str(month) + '/' + str(day))

def index(request):
    #/turnTo
    print(reverse('index:turnTo'))
    #'/'
    return redirect(reverse('index:mydate', args=[2019,12,12]))
#templates的index.htm
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>
<body>
    <span>Hello World!!</span>
</body>
</html>
```







##### 附录







#### 四、FBV视图

###### render返回响应内容

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义首页的路由
    path('', views.index, name='index'),
]
#index的views.py
#def index(request):
##    html = '<h1>Hello World</h1>'
	#status默认为200	
#    return HttpResponse(html, status=200)

#def index(request):
#    value = {'title': 'Hello MyDjango'}
#    return render(request, 'index.html', context=value)

def index(request):
    title = {'key': 'Hello MyDjango'}
    content = {'key': 'This is MyDjango'}
    #locals()返回了index函数的所有局部变量，eg：title和content
    return render(request, 'index.html', locals())
#templates的index.html
<!DOCTYPE html>
<html>
<body>
{#<h3>{{ title }}</h3>#}
    #插值语法
<h3>{{ title.key }}</h3>
<div>{{ content.key }}</div>
</body>
</html>
```



###### 设置301与302重定向

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义首页的路由
    path('', views.index, name='index'),
    # 定义商城的路由
    path('shop', views.shop, name='shop')
]
#index的views.py
def index(request):
    #permanent为True则为301，为False则为302
    #'index:shop'为http://127.0.0.1:8000/shop
    return redirect('index:shop' ,permanent=True)
    # 设置302的重定向
    # url = reverse('index:shop')
    # return HttpResponseRedirect(url)
    # 设置301的重定向
    # return HttpResponsePermanentRedirect(url)

def shop(request):
    return render(request, 'index.html')
```



###### 异常响应

```python
#设置请求异常时返回的异常页面，eg：404，505
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#因为异常的响应页面是所有应用所共有的，所以异常响应配置视图函数在根目录的urls.py中
# 全局404页面配置
handler404 = 'index.views.pag_not_found'
# 全局500页面配置
handler500 = 'index.views.page_error'

#index的urls.py
urlpatterns = [
    # 定义首页的路由
    path('', views.index, name='index'),
]

#index的views.py
def index(request):
    if request.GET.get('error', ''):
        raise Http404("page does not exist")
    else:
        return render(request, 'index.html')

def pag_not_found(request):
    """全局404的配置函数 """
    return render(request, '404.html', status=404)


def page_error(request):
    """全局500的配置函数 """
    return render(request, '500.html', status=500)
#templates的index.html
```



###### 文件下载

```python
#总而言之把D盘的放的图片下载到了浏览器显示

#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义首页的路由
    path('', views.index, name='index'),
    path('download/file1', views.download1, name='download1'),
    path('download/file2', views.download2, name='download2'),
    path('download/file3', views.download3, name='download3'),
]
#index的views.py
def index(request):
    return render(request, 'index.html')

def download1(request):
    file_path = 'D:\cat.jpg'
    try:
        r = HttpResponse(open(file_path, 'rb'))
        r['content_type'] = 'application/octet-stream'
        r['Content-Disposition'] = 'attachment; filename=cat.jpg'
        return r
    except Exception:
        raise Http404('Download error')

def download2(request):
    file_path = 'D:\duck.jpg'
    try:
        r = StreamingHttpResponse(open(file_path, 'rb'))
        r['content_type'] = 'application/octet-stream'
        r['Content-Disposition'] = 'attachment; filename=duck.jpg'
        return r
    except Exception:
        raise Http404('Download error')

def download3(request):
    file_path = 'D:\dog.jpg'
    try:
        f = open(file_path, 'rb')
        r = FileResponse(f, as_attachment=True, filename='dog.jpg')
        return r
    except Exception:
        raise Http404('Download error')
#templates的index.html
<!DOCTYPE html>
<html>
<body>
<a href="{% url 'index:download1' %}">HttpResponse-下载</a>
<br>
<a href="{% url 'index:download2' %}">StreamingHttpResponse-下载</a>
<br>
<a href="{% url 'index:download3' %}">FileResponse-下载</a>
</body>
</html>
```



###### 获取请求信息

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义首页的路由
    path('', views.index, name='index'),
]
#index的views.py
def index(request):
    # 使用method属性判断请求方式
    if request.method == 'GET':
        # 类方法的使用
        print(request.is_secure())
        print(request.is_ajax())
        print(request.get_host())
        print(request.get_full_path())
        print(request.get_raw_uri())
        # 属性的使用
        print(request.COOKIES)
        print(request.content_type)
        print(request.content_params)
        print(request.scheme)
        # 获取GET请求的请求参数
        #request.GET.get('user', '')没找到则返回空
        print(request.GET.get('user', ''))
        return render(request, 'index.html')
    elif request.method == 'POST':
        # 获取POST请求的请求参数
        print(request.POST.get('user', ''))
        #index.html是空的
        return render(request, 'index.html')
#templates的index.html
<!DOCTYPE html>
<html>
<body>
<h3>Hello world</h3>
<form action="" method="POST">
    {% csrf_token %}
    <input type="text" name="user"/>
    <input type="submit" value="提交"/>
</form>
</body>
</html>
```



###### 文件上传

原理：文件以二进制的格式读取并写入网站指定的文件夹里

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('', views.upload, name='uploaded'),
]
#index的views.py
def upload(request):
    # 请求方法为POST时，执行文件上存
    if request.method == "POST":
        # 获取上传的文件，如果没有文件，则默认为None
        myFile = request.FILES.get("myfile", None)
        if not myFile:
            return HttpResponse("no files for upload!")
        # 打开特定的文件进行二进制的写操作
        #myFile.name获取上传文件的文件名
        f = open(os.path.join("D:\\upload", myFile.name), 'wb+')
        # 分块写入文件
        for chunk in myFile.chunks():
            f.write(chunk)
        f.close()
        return HttpResponse("upload over!")
    else:
        # 请求方法为GET时，生成文件上存页面
        return render(request, 'upload.html')
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
<body>
<!--enctype要设为"multipart/form-data"-->
<form enctype="multipart/form-data" action="" method="post">
   {% csrf_token %}
   <input type="file" name="myfile" />
   <br>
   <input type="submit" value="上存文件"/>
</form>
</body>
</html>
```

![result](../AppData/Roaming/Typora/typora-user-images/image-20231015135026243.png)



######  *Cookie反爬虫

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('', views.index, name='index'),
    path('create', views.create, name='create'),
    path('myCookie', views.myCookie, name='myCookie'),
]
#index的views.py
def index(request):
    return render(request, 'index.html')

def create(request):
    r = redirect(reverse('index:index'))
    # 添加Cookie
    # response.set_cookie('uid', 'Cookie_Value')
    r.set_signed_cookie('uuid', 'id', salt='MyDj', max_age=10)
    return r

def myCookie(request):
    cookieExist = request.COOKIES.get('uuid', '')
    if cookieExist:
        # 验证加密后的Cookies是否有效
        try:
            request.get_signed_cookie('uuid', salt='MyDj')
        except:
            raise Http404('当前Cookie无效哦！')
        return HttpResponse('当前Cookie为：' + cookieExist)
    else:
        raise Http404('当前访问没有Cookie哦！')
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
<body>
<h3>Hello Cookies</h3>
<a href="{% url 'index:create' %}">创建Cookie</a>
<br>
<a href="{% url 'index:myCookie' %}">查看Cookie</a>
</body>
</html>
```



###### *请求头反爬虫

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('', views.index, name='index'),
    path('getHeader', views.getHeader, name='getHeader')
]
#index的views.py
def index(request):
    return render(request, 'index.html')

# API接口判断请求头
def getHeader(request):
    header = request.META.get('HTTP_SIGN', '')
    if header:
        value = {'header': header}
    else:
        value = {'header': 'null'}
    return JsonResponse(value)
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
{% load static %}
<script src="{% static 'jquery.js' %}"></script>
<script>
$(function(){
  $("#bt01").click(function(){
      #$===jQuery
    var value = $.ajax({
        type: "GET",
        url:"/getHeader",
        dataType:'json',
        async: false,
        headers: {
            "sign":"123",
        }
        }).responseJSON;
        value = '<h2>'+value["header"]+'</h2>';
        $("#myDiv").html(value);
    });
});
</script>
<body>
<h3>Hello Headers</h3>
<div id="myDiv"><h2>AJAX获取请求头</h2></div>
<button id="bt01" type="button">改变内容</button>
</body>
</html>
```





##### 附录







#### 五、CBV视图

###### 数据显示图

将后台的数据展示到网页上

- **RedirectView**用于实现HTTP重定向，默认情况下只定义GET请求的处理方法

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('', index, name='index'),
    path('turnTo', turnTo.as_view(), name='turnTo')
]
#index的views.py
def index(request):
    return render(request, 'index.html')

class turnTo(RedirectView):
    permanent = False
    url = None
    pattern_name = 'index:index'
    #是否将当前路由地址的请求参数传递到重定向的路由地址
    query_string = True

    #根据属性pattern_name所指向的路由生成相应的路由地址
    #http://127.0.0.1:8000/?k=1
    def get_redirect_url(self, *args, **kwargs):
        print('This is get_redirect_url')
        return super().get_redirect_url(*args, **kwargs)

    #触发HTTP的GET请求所执行的相应处理，就是是个get的请求都要路过一下这个函数
    def get(self, request, *args, **kwargs):
        print(request.META.get('HTTP_USER_AGENT'))
        return super().get(request, *args, **kwargs)
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
<body>
<h3>Hello RedirectView</h3>
    <!--href=http://127.0.0.1:8000/turnTo?k=1-->
<a href="{% url 'index:turnTo' %}?k=1">ToTurn</a>
</body>
</html>
```

- **TemplateView**是视图类的基础视图，可将数据传递给HTML模板，默认情况下只定义GET请求的处理方法

  ```python
  #MyDjango的urls.py
  urlpatterns = [
      # 指向index的路由文件urls.py
      path('', include(('index.urls', 'index'), namespace='index')),
  ]
  #index的urls.py
  urlpatterns = [
      # 定义路由
      path('', index.as_view(), name='index'),
  ]
  #index的views.py
  class index(TemplateView):
      #模板的存放位置
      template_name = 'index.html'
      #模板的引擎
      template_engine = None
      #响应的数据格式
      content_type = None
      #给模板写入一点数据，会显示到网页上，毕竟是给“模板写一点数据”
      extra_context = {'title': 'This is GET'}
  
      # 重新定义模版上下文的获取方式
      #把模板的数据得到然后又加了一点数据context['value']='I am MyDjango'
      #数据都是有返回的，可能隐藏在某个函数里返回
      def get_context_data(self, **kwargs):
          context = super().get_context_data(**kwargs)
          context['value'] = 'I am MyDjango'
          return context
  
      # 定义HTTP的POST请求处理
      #post请求路过都要经过的函数
      def post(self, request, *args, **kwargs):
          #修改了extra_context
          self.extra_context = {'title': 'This is POST'}
          #调用get_context_data
          context = self.get_context_data(**kwargs)
          return self.render_to_response(context)
  ```

  ```html
  <!--templates的index.html-->
  <!DOCTYPE html>
  <html>
  <body>
  <h3>Hello,{{ title }}</h3>
  <div>{{ value }}</div>
  <br>
  <form action="" method="post">
      {% csrf_token %}
      <input type="submit" value="Submit">
  </form>
  </body>
  </html>
  ```

- **ListView**是在TemplateView的基础上将数据以列表显示，通常将某个数据表的数据以列表表示

总而言之是将数据库的数据以列表的形式呈现出来，所以要定义模型文件

数据库迁移命名：

python manage.py makemigrations

python manage.py migrate

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('', index.as_view(), name='index'),
]
#index的models.py
class PersonInfo(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=20)
    age = models.IntegerField()
#index的views.py
class index(ListView):
    # 设置模版文件
    template_name = 'index.html'
    # 设置模型外的数据，在模型外添加数据
    extra_context = {'title': '人员信息表'}
    # 查询模型PersonInfo
    queryset = PersonInfo.objects.all()
    # 每页的展示一条数据
    paginate_by = 1
    # 如不设置，则模版上下文默认为personinfo_list
    #就是给查出来的模型起个名字，方便在模板里用
    # context_object_name = 'personinfo'
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
<body>
    <h3>{{ title }}</h3>
    <table border="1">
        <!--以列表的形式返回数据表-->
      {% for i in personinfo_list %}
          <tr>
            <th>{{ i.name }}</th>
            <th>{{ i.age }}</th>
          </tr>
      {% endfor %}
    </table>
    <br>
    <!--？？？？？？-->
    {% if is_paginated %}
    <div class="pagination">
        <span class="page-links">
            {% if page_obj.has_previous %}
                <a href="/?page={{ page_obj.previous_page_number }}">上一页</a>
            {% endif %}
            {% if page_obj.has_next %}
                <a href="/?page={{ page_obj.next_page_number }}">下一页</a>
            {% endif %}
            <br>
            <br>
            <span class="page-current">
                第{{ page_obj.number }}页,共{{ page_obj.paginator.num_pages }}页。
            </span>
        </span>
    </div>
    {% endif %}
</body>
</html>
```

- **DetailView**是在TemplateView的基础上将数据详细显示，通常获取数据表的单条数据

也和模型有关

```python
#MyDjango的urls.py
urlpatterns = [
    # 指向index的路由文件urls.py
    path('', include(('index.urls', 'index'), namespace='index')),
]
#index的urls.py
urlpatterns = [
    # 定义路由
    path('<pk>/<age>.html', index.as_view(), name='index'),
]
#index的models.py
class PersonInfo(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=20)
    age = models.IntegerField()
#index的views.py
class index(DetailView):
    # 设置模版文件
    template_name = 'index.html'
    # 设置模型外的数据
    extra_context = {'title': '人员信息表'}
    # 设置模型的查询字段
    slug_field = 'age'
    # 设置路由的变量名，与属性slug_field实现模型的查询操作
    slug_url_kwarg = 'age'
    pk_url_kwarg = 'pk'
    # 设置查询模型PersonInfo
    model = PersonInfo
    # 属性queryset可以做简单的查询操作
    # queryset = PersonInfo.objects.all()
    # 如不设置，则模版上下文默认为personinfo
    # context_object_name = 'personinfo'
    # 是否将pk和slug作为查询条件
    # query_pk_and_slug = False
```

```html
<!--templates的index.html-->
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
<body>
    <h3>{{ title }}</h3>
    <table border="1">
      <tr>
        <th>{{ personinfo.name }}</th>
        <th>{{ personinfo.age }}</th>
      </tr>
    </table>
    <br>
</body>
</html>
```



###### *数据操作视图

- FromView
- CreateView
- UpdateView
- DeleteView



###### *日期帅选视图

- MonthArchiveView
- WeekArchiveView





##### 附录







#### 六、模板

###### *django引擎

###### jinja2引擎





##### 附录







#### 七、数据库

###### 配置

```python
#项目目录/setting.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```



###### 字段类型

- BooleanField()/tinyint

- CharField()/varchar

  必须指定max_length参数值

- DateField()/date

  auto_now:每次保存自动设置该字段当前的时间

  auto_now_add:对象第一次被创建时设置时间

  default:设置当前时间

- DateTimeField()/datetime
- FloatField()/double
- DecimalField()/decimal

max_digits:总位数

decimal_places:小数点后面的位数

- EmailField()/varchar

对传入的数据有格式的要求

- IntegerField()/int
- ImageField()/varchar

保存的是图片的路径，不是保存的图片本身

- TextField()/longtext



###### 字段选项

- primary_key：主键，如果指定一个字段为主键，系统就不会设置id
- blank：字段可以为空
- null：允许该列值为空
- default：默认值，没值时使用default
- db_index：添加索引
- unique：表示该字段在数据库中的值必须是是唯一的
- db_column：指定字段名，类属性名可能和数据表的字段名不一样
- verbose_name：在admin上显示的名称



###### Meta类

可以对模型赋予一些操作

- db_table：改表名

- verbose_name：修改在admin页面显示的名字
- verbose_name_plural：修改复数的名字



###### __str__

- __str__方法用于返回对象的描述信息，如果不使用__str__方法，直接print，或者return，返回的是对象的内存地址，类似如to_String，改变对象的值，改变一部分查询结果的格式


```python
zhangsan = Student("张", 2200)
print(zhangsan) # 如果Student这个class类，没有写__str__方法，输出的是内存地址，但是加上__str__就是值
```

- 决定模型在admin默认显示的格式



###### 创建模型

```python
项目目录/app目录/models.py
class PersonInfo(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=20)
    age = models.IntegerField()
    hireDate = models.DateField()

    def __str__(self):
        return self.name

    class Meta:
        #在admin页面设置字段的显示名字
        verbose_name = '人员信息'


class Vocation(models.Model):
    id = models.AutoField(primary_key=True)
    job = models.CharField(max_length=20)
    title = models.CharField(max_length=20)
    payment = models.IntegerField(null=True, blank=True)
    #外键
    name = models.ForeignKey(PersonInfo, on_delete=models.Case)

    #设置模型的返回值
    def __str__(self):
        return str(self.id)

    class Meta:
        verbose_name = '职业信息'
```



###### 数据库迁移

**前要**：第一次迁移前看一下user表结构更改（？？？）

- 生成脚本文件：python3 manage.py makemigrations

首次执行makemigrations指令时，Django就在每个App的migrations文件夹里创建0001_initial.py文件，0001_initial.py文件将models.py定义的模型生成数据表的脚本代码

![0001_initial.py](../AppData/Roaming/Typora/typora-user-images/image-20231017155203065.png)

- 执行脚本文件(操作数据库)：python3 manage.py migrate

该文件的脚本代码可被migrate指令执行，migrate指令会根据脚本代码的内容在数据库里创建相应的数据表

注意：

- 比如数据库表单已经生成，需要删除一个字段可以直接再次执行上诉命令；
- 添加字段必须要为其设置默认值，否则django会进行询问（可写入默认值，可退出该操作）
- 



###### 数据的导入导出

- 导出

python manage.py dumpadata>data.json

- 导入

python manage.py loaddata data.json

Installed 44 object(s) from 1 fixture(s)



###### Django Shell

python3 manage.py shell

注意：代码发生变化要重新进入shell







#### 八、ORM

###### 创建

```python
from .models import Article
#方案一
Article.objects.create(title="My first article", body="My first article body")
#方案二
a = Article(title="My first article")
a.body=("My first article body")
a.save()
```



###### 删除

- 删一条

```python
from .models import Article
Article.objects.get(title="java").delete() 
```

- 删多条(条件满足的)

```python
from .models import Article
Article.objects.filter(title="java").delete() 
```

- 伪删除

is_active是否显示在页面上

全部的数据设置is_active=True,删除则将is_active=False



###### 更新

注意：传id有两种方式

/id

?id=xxx

- 单个更新

get()得到，对象.属性修改，对象.save()保存

- 批量更新

all()或者filter()查询多条数据，对象.update()修改



###### 查询

- **查询方法：**

eg：a = Book.object.[all()]

**all()**全部记录

输出a返回str规定的类型，否则默认为内存地址

**values()**指定的记录

输出a返回字典

**values_list()**指定的记录

输出a返回元组

**order_by(‘列’)**升降排序

- **查询条件：**

**filter()**符合条件的多条记录

__str__()

**exclude()**符合条件之外的全部记录



**get()**符合条件的单一记录

- **查询谓词：**

__exact：等值匹配

__contains：包含指定值

__startswith：以开头

__endswith：以结尾

__gt：大于

__gte：大于等于

__lt：小于

__lte：小于等于

__in：是否在指定范围内

__range：是否在指定区域内



###### *开发个人ORM框架

```python
# 模型字段的基本类

class Field(object):
    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type
    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)

# 模型字段的字符类型

class StringField(Field):
    def __init__(self, name):
        super().__init__(name, 'varchar(100)')

# 模型字段的整数类型

class IntegerField(Field):
    def __init__(self, name):
        super().__init__(name, 'bigint')

# 定义元类ModelMetaclass，控制Model对象的创建

class ModelMetaclass(type):
    def __new__(cls, name, bases, attrs):
        mappings = dict()
        for k, v in attrs.items():
            # 保存类属性和列的映射关系到mappings字典
            if isinstance(v, Field):
                print('Found mapping: %s==>%s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            # 将类属性移除，使定义的类字段不污染User类属性
            attrs.pop(k)
        # 假设表名和为类名的小写,创建类时添加一个__table__类属性
        attrs['__table__'] = name.lower()
        # 保存属性和列的映射关系，创建类时添加一个__mappings__类属性
        attrs['__mappings__'] = mappings
        return super().__new__(cls, name, bases, attrs)

# 定义Model类

class Model(metaclass=ModelMetaclass):
    def __init__(self, *args, **kwargs):
        self.kwargs = kwargs
        super().__init__()

    def save(self):
        fields, params = [], []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append(str(self.kwargs[k]))
        sql = 'insert into %s (%s) values (%s)'
        sql = sql % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)

# 定义模型User

class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')

# 创建实例对象

u = User(id=123, name='Dj', email='Dj@dd.gg', password='111')

# 调用save()方法

u.save()
```





#### 九、F对象

###### F (‘列名’)批量增加

```
Book.object.all().update(market_price=F('market_price')+10)
```



###### 优化多人同时进行同一个操作，简而言之上锁

```python
#点击一次like加一
def add_like(request,topic_id):
    topic=Topic.object.get(id=topic_id)
    #new_like=topic.like+1
    #topic_like=new_like
    topic.like=F('like')+1
	topic.save()
```



###### 批量的比较同一张表的两个字段

```python
books=Book.object.filter(market_price_gt=F('price'))
```

#### 十、 Q对象

###### 逻辑操作

```python
Book.object.filter(Q(price_lt=20)&Q(pub="xxx出版社"))
Book.object.filter(Q(price_lt=20)|Q(pub="xxx出版社"))
Book.object.filter(Q(price_lt=20)&~Q(pub="xxx出版社"))
```



#### 十一、*聚合查询

#### 十二、*原生数据库操作





#### 十三、Admin

###### 创建超级账户

python3 manage.py createsuperuser



###### 组

拥有统一权限的一群账号



###### 权限

- 有效

此账号是否能用

- 人员状态

此账号是否能登录管理后台



- 超级用户状态

此账号是否是超级账户



###### 注册自定义模型

模型类的名字和数据表以及admin显示有关

eg：Book

数据表：应用名_book

admin数据库下显示：Book

```python
#根目录的admin.py
#注册模型Book
admin.site.register(Book)
```



###### 模型管理器类

可以自定义admin界面的功能，所谓个性化admin页面

```python
#根目录的admin.py
class BookManager(admin.ModelAdmin):
    #列表页显示的字段名,会改变__str__()设置的格式
    list_display = ['id','title','pub','price']
    #设置修改的链接是从哪个字段进入
    list_display_links = ['title']
    #设置一个分类，快速帅选的小框在旁边
    list_filter = ['pub']
	#添加搜索框，使用模糊查询
    search_filed = ['title']
    #使字段可以在列表页面直接修改
    list_editable = ['price']
admin.site.resgter(Book,BookManager)
```







#### 十四、关系映射

###### 删除

OneToOneField、ForeignKey必须指定on_delete模式

on_delete模式
models.CASCADE：一起删除

models.PROTECT：删除时会报异常

SET_NULL：删除后，外键指定为null

SET_DEFAULT:删除后，外键值为默认值



###### 一对一

外键字段名称为‘’类属性名_id”

```python
#创建模型类
class Author(models.Model):
    name = models.CharField('作家',max_length=50)
    
class Wife(model.Model):
    name = models.CharField("妻子",max_length=50)
    author = models.OneToOneField(Author,on_delete=models.CASCADE)
    
#创建数据
#方案一：
a1 = Author.object.create(name='wang')
w1 = Wife.object.create(name='wangfuren',author=a1)
#方案二：
w1 = Wife.object.create(name='wangfuren',author_id=1)
w1 = Wife.object.create(name='wangfuren',author_id=a1.id)

#正向查询
#通过Wife查找author
wife = Wife.object.get(name='王夫人')
print(wife.name,'对应的是',wife.author.name)

#反向查询
#django内置反向属性
author1 = Author.object.get(name='王老师')
author1.wife.name
```



###### 一对多

```python
#创建模型类
class Publisher(models.Model):
    name = models.CharField('出版社名称',max_length=50)
    
class Book(models.Model):
    title = models.CharField('书名',max_leength=50)
    publisher = models.ForeignKey(Publisher,on_delete=models.CASCADE)
    
#创建数据
pub1 = Publisher.object.create(name='a')
Book.object.create(title='C++',publisher=pub1)
Book.object.create(title='Java',publisher_id=1)

#正向查询
abook = Book.object.get(id=1)
print(abook.title,abook.publisher.name)

#反向查询
pub1 = Publisher.object.get(name='a')
books = pub1.book_set.all()
books = pub1.book_set.filter(publisher=pub1)
```



###### 多对多

```python
#创建模型类
class Author(models.Model):
    name = models.CharField('姓名',max_length=50)
    
class Book(models.Model):
	title = models.CharField('书名',max_length=50)
	authors = models.ManyToManyField(Author)
    
#创建数据
#方案一：
author1 = Author.object.create(name='a')
author2 = Author.object.create(name='b')

book11 = author1.book_set.create(title='c')
author2.book_set.add(book11)

#方案二：


#正向查询

#反向查询
```



#### 十五、Cookies和Session

###### Cookie存储技术

长期：浏览器存Cookies

```python
#不指定max_age,expires时关闭浏览器数据失效
HttpResponse.set_cookie(key,value='',max_age=None(S),expires=None)
HttpResponse.delete_cookie(key)
value = request.COOKIE.get(key,'默认值')
```



###### Session存储技术

短期：服务器的数据表存Session，只能删除过期的Session记录

```python
request.session['KEY'] = VALUE
value = request.session['KEY']
value = request.session.get['KEY','默认值']
del request.session['KEY']
```

settings.py配置

```python
#保存时长
SEESION_COOKIE_AGE = 60*60*24*7*2
#设置浏览器关闭session失效，默认为False
SESSION_EXPIRE_AT_BROWSER_CLOSE = True
#删除过期的Session数据
python3 manage.py clearsessions
```







#### 十六、md5

- 定长输出32

- 不可逆
- 雪崩效应
- 相同明文密码的md5相同
- 处理密码，文件完整性校验







#### 十七、项目

###### 云笔记项目

- 注册

应用没创建不要配置应用路由

链接mysql要myclient或者pymysql

创建应用yu路由

创建模型类与数据表

创建HTML模板

刷新是post请求，执行相同的请求

- 登录





#### 十八、常见问题

- 执行python3 manage.py makemigrations异常，缺少默认值

隐式直接加值

显示退出到model.py文件添加默认值

- 某种原因造成生成的xxx.py文件不对应

删除所有的xxx.py文件与数据库，重新创建数据库

最后进行迁移





#### 附录

/：经常代表http://127.0.0.1:8000



缓存

中间件

# Django

## install

### install python

### set up a database（设置数据库）

### install Django

- install with pip
> pip install Django
- 查看安装版本

```python
>>> import django
>>> print(django.get_version())
2.1
```

## start Django app

### Creating a project

> django-admin startproject mysite
- 这将在当前目录下创建一个 mysite 文件夹作为项目的根目录
- 初始目录结构

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

- 外层的mysite/：是项目的容器，可以为任意名字。
- manage.py：一种让你可以使用各种方式管理Django项目的命令行工具。在mysite/目录下输入python3 manage.py help，看一看它都能做什么。
- 内层的mysite/：包含项目，是一个纯Python包。你可以在包里调用它内部的任何东西。
- __init__.py：一个空文件，告诉Python这个目录应该被认为是一个Python包。一般，你不需要去修改它。
- settings.py：Django项目的配置文件。
- urls.py：Django项目的URL声明。
- wsgi.py：作为项目的运行在WSGI兼容的Web服务器的入口。

### The development server

```bash
$ python manage.py runserver

Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

February 11, 2019 - 15:50:53
Django version 2.1, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
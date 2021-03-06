django 是 Python 下的一个 web 框架，了解一下 Python,顺便看看 django


###python

```
#!/user/bin/python
# -*- coding: UTF-8 -*-

import sys

print sys.argv

# 多行语句
total = item_one + \
        item_two + \
        item_three
        
# 引号
word = 'word'
sentence = "这是一个句子"
paragraph = """这是一个段落。
包含了多个语句"""

# 一行多个语句使用 ；隔开
print "a"; print "b"

# print 不换行后面加 ,
print x,
print y

```

### 依赖管理

```
# 链接指向的内容保存为文件
curl -O https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py 
sudo pip install django
```

### django

```
$ django-admin startproject djangoApp
$ python manage.py migrate
$ python manage.py runserver
$ python manage.py runserver 127.0.0.1:8000

Starting development server at http://127.0.0.1:8000/
```

#### HelloWorld

在djangoApp下面 urls.py 同级目录里创建一个 home.py

```
from django.http import HttpResponse

def hello(request):
	return HttpResponse("Hello world")
```

要想这个hello 被外界调用到，还需要在 urls.py 里进行注册

```
# 导入文件
from . import home

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # ^$中间代表path,没有path也可以
    url(r'^$',home.hello),
]

```
启动server(```python manage.py runserver```)后,访问 http://127.0.0.1:8000/ 可以看到 'hello world'

### 接收上传的文件

创建 myfile.py 

```
# -*- coding: utf-8 -*-

from django.http import HttpResponse
import os
import sys  

reload(sys)
sys.setdefaultencoding('utf8')

def upload_file(request):
    if request.method == 'POST':
        print request.FILES
        obj = request.FILES.get('files')
        f = open(os.path.join('upload',obj.name),'wb')
        for line in obj.chunks():
            f.write(line)
        f.close()
        return HttpResponse('{"msg":"上传成功"}')
    else:
        return HttpResponse('{"msg":"上传失败"}')

def handle_uploaded_file(f):
    with open(f.name, 'wb+') as destination:
        for chunk in f.chunks():
            destination.write(chunk)
```

需要在项目根目录下创建一个文件夹叫 "upload",否则保存文件会出错。

myfile同样需要在 urls.py 里面注册

```
from . import home,myfile

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$',home.hello),
    url(r'^myfile$',myfile.upload_file),
]

```

客户端调用代码

```
[self upLoadFileWithUrl:@"http://127.0.0.1:8000/myfile" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
    [formData appendPartWithFileData:[@"wwwwerwerwer" dataUsingEncoding:NSUTF8StringEncoding] name:@"files" fileName:@"file" mimeType:@"text/plain"];
} uploadProgressBlock:^(NSProgress * _Nonnull progress) {
    
} complateBlock:^(XNOConnectData * _Nonnull connectData, NSURLSessionTask * _Nonnull task) {
    
}];
```

上传成功后，可以看到upload下面都一个名为file的文件，文件的内容就是一个行字符串，"wwwwerwerwer"。



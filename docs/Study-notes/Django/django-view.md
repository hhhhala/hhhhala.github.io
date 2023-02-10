# django视图层

#### 三个试图函数的返回方法

HttpResponse():返回字符串类型的数据<br>
render():返回html文件<br>
reidirect():重定向，可以写完整的网址或者只写一个后缀
![shortcuts](img/shortcuts.png)



#### JsonResponse

1、不进行json转换传递字典给前端
```	python
from django.shortcuts import render, HttpResponse
import Json
def index(request):
    dict1 = {'username': 'hhhh', 'password':'123456'}
    return HttpResponse(dict1)
```
![python-data](img\python-data.png)<br>
2、导入json模块使用dunps转换数据传递给前端
```	python
from django.shortcuts import render, HttpResponse
import json
def index(request):
    dict1 = {'username': 'hhhh', 'password':'123456'}
    json_str = json.dumps(dict1, ensure_ascii=False)	# 将python数据转换为JSON字符串,ensure_ascii取消转码
    return HttpResponse(json_str)
```
![json-data](img\json-data.png)<br>
3、导入JsonResponse模块进行转换数据传递给前端

```	python
from django.shortcuts import render, HttpResponse
from django.http import JsonResponse
def index(request):
    dict1 = {'username': 'hhhh', 'password':'123456'}
    return JsonResponse(dict1)
```
![JsonResponse](img\JsonResponse.png)<br>
**form表单上传文件以及后端获取**<br>
1.method必须指定位post<br>
2.enctype必须换成multiparty/form-data<br>
3.post请求时需要将setting中middleware第四句注释<br>

enctype就是encodetype就是编码类型的意思<br>
multipart/form-data是指表单数据由多个部分构成，既有文本数据，又有文件等二进制格式的数据<br>
默认情况下，enctype的值时’application/x-www-form-urlencoded‘,不能用于文件上传只能上传文本格式的数据
![multipartform-data](img\multipartform-data.png)
![request.FILES](img\request.FILES.png)
![multiValueDict](img\multiValueDict.png)

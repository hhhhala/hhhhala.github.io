# django-数据库

## models.py操作
**models.CharField(max_length=32)：**存放字符串字段<br>
**models.IntegerField()：**存放整形字段<br>
**models.DateField()和DatetimeField()：**存放年月日<br>
	*两个重要参数*<br>
		auto_now:每次操作数据时，该字段自动更新当前事件<br>
		auto_now_add:在创建数据时候自动创建时间记录，之后手动修改才会改变<br>

## 数据库操作方法：
**all()：**查询所有的数据<br>
**get()：**直接拿数据对象，不存在则报错<br>
**filter()：**带有过滤条件的查询 - 返回queryset对象<br>
**first()：**拿queryset里面的第一个元素<br>
**last()：**拿queryset里面的最后一个元素<br>

```python
	models.User.objects.filter(name='ha').first()
	models.User.objects.filter(name='ha').last()
```
**values()：**获取指定的数据字段 - 字典·<br>
**values_list()：**获取指定数据字段 - 元组<br>
```python
	models.User.objects.values('name', 'age')				# {'haha', 21}
	models.User.objects.values_list('name', 'age')			# ('haha', 21)
```
**distinct()：**去重 - 一定是一模一样的数据，只要有一个字段不同都不会去掉<br>
**order_by()：**排序 - 默认升序(ASC) - 在过滤条件前添加'-'则为降序<br>
```python
	models.User.objects.order_by('age')		# 升序
	models.User.objects.order_by('-age')	# 降序
```
**reverse()：**反转 - 将升序反转为降序<br>
```python
	models.User.objects.order_by('age').reverse()	# 降序
```
**count()：**统计当前数据的个数<br>
**exclude()：**排除查询<br>
```python
	models.User.objects.exclude(name='haha')	# 获取除了name为haha的数据
```
**exists()：**判断某个对象是否存在

## 单表操作
#### 增加
##### 方法一，通过create()直接增加
```python
	models.User.objects.create(id=6, name='haha', age=25, register_time=ctime)       
    # 返回值，创建对象本身
```
##### 方法二，实例化对象，再通过save()保存到数据库
```python
	user_obj = models.User(name='ag3a', age=1232, register_time=ctime)                   
    user_obj.save()
```

#### 删除
##### delete()方法直接删除
```python
	# pk表示当前表的主键
    # filter() - 符合条件的所有数据，返回列表
    # first() - 获取filter返回列表的第一个对象
    models.User.objects.filter(name='haha').delete()            # 批量删除
    models.User.objects.filter(name='ha').first().delete()      # 单个删除
```

#### 修改
##### 方法1 upadate()
```python
    models.User.objects.filter(pk=9).update(name='mujin')
```
##### 方法2 实例化对象，通过save方法保存到数据库

```python
    # get方法可以直接返回数据对象，但是不存在则保存，filter不会
        user_obj = models.User.objects.get(pk=4)
        user_obj.name = 'muns'
        user_obj.save()
```
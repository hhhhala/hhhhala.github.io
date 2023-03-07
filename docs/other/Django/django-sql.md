# django-数据库

## models.py操作

#### 字段
**models.AutoField():**自动增长的IntegerField.通常不指定，django会自动创建一个<br>
**models.BooleanField():**布尔值字段，值为True或False<br>
**models.NullBooleanField():**值为Null，True或False<br>
**models.CharField(max_length=32)：**存放字符串字段，max_length表示最大字符数<br>
**models.TextField():**存放大文本字段，一般超过几千字符的时候使用<br>
**models.IntegerField()：**存放整形字段(范围有限，不能存手机号)<br>
**models.DecimalField():**浮点数，max_digits表示总位数，decimal_places小位数<br>
**models.FloatField():**浮点型<br>
**models.DateField():**年月日<br>
**models.TimeField():**时间<br>
**models.DatetimeField()：**日期时间<br>
	*三个时间方法的两个重要参数*<br>
		auto_now:每次操作数据时，该字段自动更新当前事件<br>
		auto_now_add:在创建数据时候自动创建时间记录，之后手动修改才会改变<br>
		两个参数互斥<br>
**models.FileField():**上传文件
**models.ImageField():**继承FileField，对上传的文件进行校验，确保是图片
**models.EmailField():**邮箱字段（在数据库中就是varchar(255)）

#### 字段参数
null:表示某个字段可以为空
default:设置默认值
unique:字段唯一  -- true/false 默认false
db_column:字段名称，如果未定，则使用属性名称
db_index:若为True，则在表中为字段创建索引(下标)
primary_key:主键(True)

#### 关系字段
ForeignKey:外键，参数
	to:要关联的标，
	to_field:要关联的字段
	on_delete:当删除关联表种的数据时，当前表与关联表的行为，一般用models.CASCADE
OneToOneField:一对一字段，参数同ForeignKey
MantToMantField:多对多字段，为自动创建第三张表

## orm数据库操作方法：
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
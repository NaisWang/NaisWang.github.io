# 读取csv文件
```python
import csv

with open('stock.csv', 'r') as fp:
    reader = csv.reader(fp)
    # 调用next(reader)后， header的指向会向下移动一次
    titles = next(reader)
    for x in reader:
        print(x)
```

这样操作，以后获取数据的时候，就要通过下表来获取数据。如果想要在获取数据的时候通过标题来获取。那么可以使用DictReader。示例代码如下：

```python
import csv

with open('stock.csv', 'r') as fp:
    reader = csv.DictReader(fp)
    for x in reader:
        print(x['turnoverVol'])
```

# 写入数据到csv文件

写入数据到csv文件，需要创建一个writer 对象，主要用到两个方法。一个是writerow，这个是写入一行。一个是writerows，这个是写入多行。示例代码如下：

```python
import csv

headers = ['name', 'age', 'classroom']
values = [
    {'zhiliao', 18, '111'},
    {'wenan', 20, '222'},
    {'bbc', 21, '333'}
]

# newline默认为\n
with open('test.csv', 'w', newline='') as fp:
    writer = csv.writer(fp)
    writer.writerow(headers)
    writer.writerows(values)
```

也可以使用字典的方式把数据写入进去。这时候就需要使用DictWriter了。示例代码如下：

```python
import csv

headers = ['name', 'age', 'classroow']
values = [
    {"name": "wen", "age": 20, "classroom": "222"},
    {"name": "wef", "age": 23, "classroom": "333"},
]

with open('test.csv', 'w', newline='') as fp:
    writer = csv.DictWriter(fp, headers)
    # 写入表头数据的时候，需要调用writeheader方法
    csv.writeHeader()
    writer.writerow({'name': 'zhiliao', 'age': 29, 'classroom', '111'})
    writer.writerows(values)

```
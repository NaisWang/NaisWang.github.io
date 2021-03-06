# 简介
在 Python 中，我们可以使用内置的re模块来使用正则表达式。

有一点需要特别注意的是，正则表达式使用 对特殊字符进行转义，所以如果我们要使用原始字符串，只需加一个 r 前缀，示例：
```python
r'chuanzhiboke\t\.\tpython'
```

**re 模块的一般使用步骤如下：**
1. 使用 `compile()` 函数将正则表达式的字符串形式编译为一个 `Pattern` 对象
2. 通过 `Pattern` 对象提供的一系列方法对文本进行匹配查找，获得匹配结果，一个 Match 对象。
3. 最后使用 `Match` 对象提供的属性和方法获得信息，根据需要进行其他的操作

# compile 函数
compile 函数用于编译正则表达式，生成一个 Pattern 对象，它的一般使用形式如下：
```python
import re

# 将正则表达式编译成 Pattern 对象
pattern = re.compile(r'\d+')
```

在上面，我们已将一个正则表达式编译成 Pattern 对象，接下来，我们就可以利用 pattern 的一系列方法对文本进行匹配查找了。

Pattern 对象的一些常用方法主要有：
- match 方法：从起始位置开始查找，一次匹配
- search 方法：从任何位置开始查找，一次匹配
- findall 方法：全部匹配，返回列表
- finditer 方法：全部匹配，返回迭代器
- split 方法：分割字符串，返回列表
- sub 方法：替换

# match 方法
match 方法用于查找字符串的头部（也可以指定起始位置），它是一次匹配，只要找到了一个匹配的结果就返回，而不是查找所有匹配的结果。它的一般使用形式如下：
```
match(string[, pos[, endpos]])
```
其中，string 是待匹配的字符串，pos 和 endpos 是可选参数，指定字符串的起始和终点位置，默认值分别是 0 和 len (字符串长度)。因此，当你不指定 pos 和 endpos 时，match 方法默认匹配字符串的头部。

当匹配成功时，返回一个 Match 对象，如果没有匹配上，则返回 None。
```python
>>> import re
>>> pattern = re.compile(r'\d+')  # 用于匹配至少一个数字

>>> m = pattern.match('one12twothree34four')  # 查找头部，没有匹配
>>> print m
None

>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print m
None

>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print m                                         # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>

>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
```

在上面，当匹配成功时返回一个 Match 对象，其中：
- group([group1, …]) 方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可直接使用 group() 或 group(0)；
- `start([group])` 方法用于获取分组匹配的子串在整个字符串中的起始位置（子串第一个字符的索引），参数默认值为 0；
- `end([group])`方法用于获取分组匹配的子串在整个字符串中的结束位置（子串最后一个字符的索引+1），参数默认值为 0；
- `span([group])`方法返回 (start(group), end(group))。

再看看一个例子：
```python
>>> import re
>>> pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)  # re.I 表示忽略大小写
>>> m = pattern.match('Hello World Wide Web')

>>> print m     # 匹配成功，返回一个 Match 对象
<_sre.SRE_Match object at 0x10bea83e8>

>>> m.group(0)  # 返回匹配成功的整个子串
'Hello World'

>>> m.span(0)   # 返回匹配成功的整个子串的索引
(0, 11)

>>> m.group(1)  # 返回第一个分组匹配成功的子串
'Hello'

>>> m.span(1)   # 返回第一个分组匹配成功的子串的索引
(0, 5)

>>> m.group(2)  # 返回第二个分组匹配成功的子串
'World'

>>> m.span(2)   # 返回第二个分组匹配成功的子串
(6, 11)

>>> m.groups()  # 等价于 (m.group(1), m.group(2), ...)
('Hello', 'World')

>>> m.group(3)   # 不存在第三个分组
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: no such group
```

# search 方法
search 方法用于查找字符串的任何位置，它也是一次匹配，只要找到了一个匹配的结果就返回，而不是查找所有匹配的结果，它的一般使用形式如下：
```
search(string[, pos[, endpos]])
```
其中，string 是待匹配的字符串，pos 和 endpos 是可选参数，指定字符串的起始和终点位置，默认值分别是 0 和 len (字符串长度)。
当匹配成功时，返回一个 Match 对象，如果没有匹配上，则返回 None。
让我们看看例子：
```python
>>> import re
>>> pattern = re.compile('\d+')
>>> m = pattern.search('one12twothree34four')  # 这里如果使用 match 方法则不匹配
>>> m
<_sre.SRE_Match object at 0x10cc03ac0>
>>> m.group()
'12'
>>> m = pattern.search('one12twothree34four', 10, 30)  # 指定字符串区间
>>> m
<_sre.SRE_Match object at 0x10cc03b28>
>>> m.group()
'34'
>>> m.span()
(13, 15)
```

再来看一个例子：
```python
# -*- coding: utf-8 -*-

import re
# 将正则表达式编译成 Pattern 对象
pattern = re.compile(r'\d+')
# 使用 search() 查找匹配的子串，不存在匹配的子串时将返回 None
# 这里使用 match() 无法成功匹配
m = pattern.search('hello 123456 789')
if m:
    # 使用 Match 获得分组信息
    print 'matching string:',m.group()
    # 起始位置和结束位置
    print 'position:',m.span()
```
执行结果：
```
matching string: 123456
position: (6, 12)
```

# findall 方法
上面的 match 和 search 方法都是一次匹配，只要找到了一个匹配的结果就返回。然而，在大多数时候，我们需要搜索整个字符串，获得所有匹配的结果。
findall 方法的使用形式如下：
```
findall(string[, pos[, endpos]])
```
其中，string 是待匹配的字符串，pos 和 endpos 是可选参数，指定字符串的起始和终点位置，默认值分别是 0 和 len (字符串长度)。
findall 以列表形式返回全部能匹配的子串，如果没有匹配，则返回一个空列表。
看看例子：
```python
import re
pattern = re.compile(r'\d+')   # 查找数字

result1 = pattern.findall('hello 123456 789')
result2 = pattern.findall('one1two2three3four4', 0, 10)

print result1
print result2
```
执行结果：
```
['123456', '789']
['1', '2']
```

# finditer 方法
finditer 方法的行为跟 findall 的行为类似，也是搜索整个字符串，获得所有匹配的结果。但它返回一个顺序访问每一个匹配结果（Match 对象）的迭代器。

看看例子：
```python
# -*- coding: utf-8 -*-

import re
pattern = re.compile(r'\d+')

result_iter1 = pattern.finditer('hello 123456 789')
result_iter2 = pattern.finditer('one1two2three3four4', 0, 10)

print type(result_iter1)
print type(result_iter2)

print 'result1...'
for m1 in result_iter1:   # m1 是 Match 对象
    print 'matching string: {}, position: {}'.format(m1.group(), m1.span())

print 'result2...'
for m2 in result_iter2:
    print 'matching string: {}, position: {}'.format(m2.group(), m2.span())
```

执行结果：
```
<type 'callable-iterator'>
<type 'callable-iterator'>
result1...
matching string: 123456, position: (6, 12)
matching string: 789, position: (13, 16)
result2...
matching string: 1, position: (3, 4)
matching string: 2, position: (7, 8)
```

# split 方法
split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：
```
split(string[, maxsplit])
```
其中，maxsplit 用于指定最大分割次数，不指定将全部分割。
看看例子：
```python
import re
p = re.compile(r'[\s\,\;]+')
print p.split('a,b;; c   d')
```
执行结果：
```
['a', 'b', 'c', 'd']
```

# sub 方法
sub 方法用于替换。它的使用形式如下：
```
sub(repl, string[, count])
```
其中，repl 可以是字符串也可以是一个函数：
- 如果 repl 是字符串，则会使用 repl 去替换字符串每一个匹配的子串，并返回替换后的字符串，另外，repl 还可以使用 id 的形式来引用分组，但不能使用编号 0；
- 如果 repl 是函数，这个方法应当只接受一个参数（Match 对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。
- count 用于指定最多替换次数，不指定时全部替换。

看看例子：
```python
import re
p = re.compile(r'(\w+) (\w+)') # \w = [A-Za-z0-9_]
s = 'hello 123, hello 456'

print p.sub(r'hello world', s)  # 使用 'hello world' 替换 'hello 123' 和 'hello 456'
print p.sub(r'\2 \1', s)        # 引用分组

def func(m):
    return 'hi' + ' ' + m.group(2)

print p.sub(func, s)
print p.sub(func, s, 1)         # 最多替换一次
```

执行结果：
```
hello world, hello world
123 hello, 456 hello
hi 123, hi 456
hi 123, hello 456
```

# 匹配中文
在某些情况下，我们想匹配文本中的汉字，有一点需要注意的是，中文的 unicode 编码范围 主要在`[u4e00-u9fa5]`，这里说主要是因为这个范围并不完整，比如没有包括全角（中文）标点，不过，在大部分情况下，应该是够用的。

假设现在想把字符串 title = u'你好，hello，世界' 中的中文提取出来，可以这么做：
```python
import re

title = u'你好，hello，世界'
pattern = re.compile(ur'[\u4e00-\u9fa5]+')
result = pattern.findall(title)

print result
```
注意到，我们在正则表达式前面加上了两个前缀 ur，其中 r 表示使用原始字符串，u 表示是 unicode 字符串。

执行结果:
```
[u'\u4f60\u597d', u'\u4e16\u754c']
```

# 贪婪模式与非贪婪模式
1. 贪婪模式：在整个表达式匹配成功的前提下，尽可能多的匹配 ( * )；
2. 非贪婪模式：在整个表达式匹配成功的前提下，尽可能少的匹配 ( ? )；
3. **Python里数量词默认是贪婪的。**
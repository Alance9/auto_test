<!-- 深浅拷贝 -->

> ### 1. 应用场景

?> 多线程场景下，共享相同的数据对象时，可能会导致竞态条件。 <br>
原始数据存在嵌套结构，且被多个浅拷贝对象引用时，更新数据后，会生成脏数据。 <br>
<br>
 解决方法：选择更安全的深拷贝

> ### 1. 深浅拷贝

?> 原始对象，指被引用的对象 <br>
<br>
浅拷贝，生成新对象，引用原始对象的元素 <br>
作为引用对象，修改可变嵌套元素，影响原始对象，以及其他引用对象 <br>
修改不可变嵌套元素，原始对象、其他引用对象不受影响 <br>
<br>
深拷贝，生成新对象，复制原始对象的元素，存放到新地址 <br>
作为独立对象，修改任意元素，不影响原始对象、其他引用对象 <br>
<br>
可变嵌套元素：list 列表、dict 字典 <br>
不可变嵌套元素：tuple 元组 <br>
元祖不支持修改，修改后已经不是原元组，而是产生的新元组


```python
# 原始对象
a = ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
s = '123'

# 浅拷贝对象
ss = s
b = list(a)
c = a.copy()

# 深拷贝对象
import copy
d = copy.deepcopy(a)
```

> ### 3. 示例参考


``` python
# 深浅拷贝
import copy

a = ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
# 赋值
b = list(a)
# 浅拷贝
c = a.copy()
# 深拷贝
d = copy.deepcopy(a)

print(id(a), id(b), id(c), id(d))   
# 2004701492800 2004701510592 2004701179648 2004702140288
# ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]

# a 为原始对象，b、c 为浅拷贝对象，d 为深拷贝对象

# list 可变元素
b[2].append('5')
print(a)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(b)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(c)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}] 
b[2].remove('5')

c[2].append('5')
print(a)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(b)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(c)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}]  
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}] 
c[2].remove('5')

d[2].append('5')
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}] 
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]  
print(d)    # ['1', '2', ['3', '4', '5'], ('a', 'b'), {'c': 'd'}] 
d[2].remove('5')


# tuple 不可变元素
print('-------------------------------------------')

b[3] += ('e',)
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b', 'e'), {'c': 'd'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
b[3] = ('a','b')

c[3] += ('e',)
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b', 'e'), {'c': 'd'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
c[3] = ('a','b')

d[3] += ('e',)
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b', 'e'), {'c': 'd'}]
d[3] = ('a','b')


# dict 可变元素
print('-------------------------------------------')

b[4]['c'] = 'f'
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
b[4]['c'] = 'd'

c[4]['c'] = 'f'
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
c[4]['c'] = 'd'

d[4]['c'] = 'f'
print(a)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(b)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(c)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'd'}]
print(d)    # ['1', '2', ['3', '4'], ('a', 'b'), {'c': 'f'}]
d[4]['c'] = 'd'
```
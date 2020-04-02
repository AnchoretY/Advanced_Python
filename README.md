# Advanced_Python
Python一些比较高阶的使用技巧

### 1. Pythonic  
&emsp;&emsp;对于想要python进阶的朋友首先由要学习的就是代码风格，一手漂亮的代码是人们识别初级程序员与高级程序员最简单直接的方式。Pythonic是由Google在[《python代码风格指南》](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)中提出的，其中包含了类、函数、注释、TODO等各种python常用的功能所提倡的书写方式。需要进行学习的朋友可以直接点击[这里](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)查看完整的指南。

##### 命名
- 函数、变量、属性应该使用小写字母来进行拼写，个单词之间使用"\_"进行相连,例如:updata_data
- 受保护的实例属性应该以单个"\_"开头，例如: \_update_data
- 私有实例属性应该以两个"\_"进行开头，例如：\_\_update_data
- 类与异常应该以每个单词首字母大写的方式来进行命名，例如: UpdateData
- 模块级别的常量应该采用大写字母来进行拼写，各单词之间采用下滑线来相连，例如:LOG_DIR
- 类中实例方法应该将首个参数命名为self，表示该对象自身
- 类方法的首个参数应该命名为cls表示该类自身

##### 表达式和语句
- 不要通过长度的方式来判断列表是否为空(if len(somelist) == 0)，而应该采用if not somelist这种方式来进行判断。
- 文件中的import语句应该按顺序划分成三个部分，分别表示标准库模块、第三方模块以及自用模块，在每一部分之中各个import语句应该安模块的字幕顺序来排列

##### 空白
- 使用空格来表示缩进而不是tab
- 对于占据多行的表达式来说，除了首行之外都应该在通常的锁紧级别之上增加4个空格
- 函数与类之间使用两个空行隔开
- 同一类中各个方法之间采用一个空行隔开


### 2.可变对象与不可变对象
&emsp;&emsp;python中有一个非常重要的概念就是可变变量与不可变变量，所谓可变变量就是当对变量进行改变时，当前变量可以直接在原有地址进行改变，不需要声明重新创建新的空间来存储改编后的变量值；不可变变量与之相反，变量创建后不能进行修改，要想对变量进行修改只能开辟新的地址空间，将变量指针指向新的地址空间。下面是python中常见的可变数据类型与不可变数据类型：

**不可变数据类型**：Number（数字）、String（字符串）、Tuple（元组）  
**可变数据类型**：List（列表）、Dictionary（字典）、Set（集合）

&emsp;&emsp;具体的使用的我们可以看下面的例子：

**不可变类型**

```python
i = 77
j = 77
print(id(77))                  #140396579590760
print('i id:' + str(id(i)))      #i id:140396579590760
print('j id:' + str(id(j)))      #j id:140396579590760
print i is j                    #True
j = j + 1
print('new i id:' + str(id(i)))  #new i id:140396579590760
print('new j id:' + str(id(j)))  #new j id:140396579590736
print i is j                    #False
```

&emsp;&emsp;这里有i和j俩个变量的值为77，通过打印77的ID和变量i，j在内存中的id我们得知它们都是指向同一块内存。所以说i和j都是指向同一个对象的。然后我们修改j的值，让j的值+1.按道理j修改之后应该i的值也发生改变的，因为它们都是指向的同一块内存，但结果是并没有。因为int类型是不可变类型，所有其实是j复制了一份到新的内存地址然后+1，然后j又指向了新的地址。所以j的内存id发生了变化。

![](https://raw.githubusercontent.com/AnchoretY/images/master/blog/image.57cmchxkny8.png)

**可变类型**
```python
a = {}
b = a
print(id(a))                # 140367329543360
a['a'] = 'hhhh'
print('id a:' + str(id(a))) # id a:140367329543360
print('a:' + str(a))        # a:{'a': 'hhhh'}
print('id b:' + str(id(b))) # id b:140367329543360
print('b:' + str(b))        # b:{'a': 'hhhh'}
```
&emsp;&emsp;可以看到a最早的内存地址id是140367329543360 然后把a赋值给b其实就是让变量b的也指向a所指向的内存空间。然后我们发现当a发生变化后，b也跟着发生变化了，因为list是可变类型，所以并不会复制一份再改变，而是直接在a所指向的内存空间修改数据，而b也是指向该内存空间的，自然b也就跟着改变了。
![image](https://raw.githubusercontent.com/AnchoretY/images/master/blog/image.x30oinj2ykn.png)


### 3. 参数传递
&emsp;&emsp;**在python中只有一种参数传递传递方式：传引用，也就是传递给函数的是原变量实际所指向的内存空间，修改的时候就会根据该引用的指向去修改该内存中的内容**，所以按道理说我们在函数内改变了传递过来的参数的值的话，原来外部的变量也应该受到影响。但是上面的一点中我们说到了python中有可变类型和不可变类型，这样的话，**当传过来的是可变类型(list,dict)等可变类型时，我们在函数内部修改就会影响函数外部的变量**。而**传入的是不可变类型时在函数内部修改改变量并不会影响函数外部的变量，因为修改的时候会先复制一份再修改**。下面通过代码证明一下：

```python
def test(a_int, b_list):
    a_int = a_int + 1
    b_list.append('13')
    print('inner a_int:' + str(a_int))
    print('inner b_list:' + str(b_list))

if __name__ == '__main__':
    a_int = 5
    b_list = [10, 11]
    test(a_int, b_list)
    print('outer a_int:' + str(a_int))
    print('outer b_list:' + str(b_list))
```
&emsp;&emsp;输出为：
```python
inner a_int:6
inner b_list:[10, 11, '13']
outer a_int:5
outer b_list:[10, 11, '13']
```
&emsp;&emsp;可以明显的看出int类型传入的变量在函数内部虽然已经发生了改变，但是外部值依然没有改变；而传入的list外部变量随着函数内部变量的变化发生了改变

### 4. 字符串拼接
&emsp;&emsp;**对于大量字符串的拼接操作，尽量应该使用将字符串加入列表，然后使用join函数加入空字符串的方式进行拼接**，避免使用for循环+=或+方式进行拼接，因为字符串是不可变对象，这种方式会不断地去创建新的对象，导致二次方的运行时间。
```diff
+ items = ['<table>']
+ for last_name, first_name in employee_list:
+    items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
+ items.append('</table>')
+ employee_table = ''.join(items)

- employee_table = '<table>'
- for last_name, first_name in employee_list:
-     employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
- employee_table += '</table>'
```
### 5.迭代器  
&emsp;&emsp;迭代器是一个**可以记住遍历的位置的对象**。迭代器对象**从集合的第一个元素开始访问，直到所有的元素被访问完结束**。迭代器**只能往前不会后退**。  
&emsp;&emsp;迭代器包含两个基本方法：iter()和next()。iter()创建迭代器,字符串，列表或元组对象都可用于创建迭代器,next()输出迭代器的下一个对象。 

##### 特点
- 迭代器不可重复利用，迭代完就变成空了，再次调用会引发StopIteration异常；需要通过copy包中的deepcopy复制迭代器从而可循环使用。
- 迭代器不能回退，只能往前进行迭代；
- **Lazy evaluation**:迭代器不要求你事先准备好整个迭代过程中所有的元素。**迭代器仅仅在迭代至某个元素时才计算该元素，而在这之前或之后，元素可以不存在或者被销毁**。这个特点使得它**特别适合用于遍历一些巨大的或是无限的集合**，比如几个G的文件，或是斐波那契数列等等。这个特点被称为延迟计算或惰性求值(Lazy evaluation)；
- **迭代器更大的功劳是提供了一个统一的访问集合的接口**。只要是实现了iter()方法的对象，就可以使用迭代器进行访问。

##### 遍历
&emsp;&emsp;第一种方式就是通过不断地的**调用next()函数**来进行
```python
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2
```
&emsp;&emsp;迭代器也**可以使用for循环进行遍历**
```python
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```
##### 迭代器创建
&emsp;&emsp;迭代器的创建第一种就是使用python已经提供的**iter()函数之间使用列表、字符串创建**。
```python
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
```
&emsp;&emsp;另一种更加定制化的方式就是**自定义用户的迭代器类**。把一个类作为一个迭代器使用需要在类中实现两个方法 __iter__() 与 __next__() 。
- __iter__() 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 __next__() 方法并**通过 StopIteration 异常标识迭代的完成**。
- __next__() 方法会返回下一个迭代器对象。
&emsp;&emsp;创建一个返回数字的迭代器，初始值为 1，逐步递增 1：
```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
 
myclass = MyNumbers()
myiter = iter(myclass)
 
print(next(myiter))  # 1
print(next(myiter))  # 2
print(next(myiter))  # 3
...
print(next(myiter))  # 20
print(next(myiter))  # StopIteration异常
```

### 6. 生成器
&emsp;&emsp;在Python 中，**使用了yield**的**函数**被称为生成器（generator）。本质上生成器其实是一个特殊的函数，该函数返回的对象为一个迭代器，在调用生成器运行的过程中，**每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行**。

##### 生成器与迭代器的联系：
> 调用一个生成器函数，返回的是一个迭代器对象。

##### 生成器与迭代器的区别: 
> 生成器是函数,通过yield来进行保存状态，迭代器是一个类，通过实例参数保存状态


##### 生成器的遍历
&emsp;&emsp;由于生成器本质上就是一种特殊的迭代器，因此其遍历方式也与迭代器一样。

##### 生成器创建
&emsp;&emsp;生成器最常见的创建方式就是直接自定义生成式函数，以斐波那契数生成器为例：
```python
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```
&emsp;&emsp;还有另一种与列表生成式非常相似的生成器创建方式，只需要将列表生成式的“[]”换成“()”,下面为使用实例：
```python
generator_ex = (x*x for x in range(10))
print(generator_ex)       

output:
    <generator object <genexpr> at 0x7f2476a93c78>
```

### 7. 装饰器


### 8. 使用生成器替代列表生成式
&emsp;&emsp;再初学者使用python中最常用到的数据结构就是列表了，但是列表存在缺点：创建列表对象时会一次性将全部数据放入内存，导致当数据量很大时很容易造成内存不足、无效的内存占用的问题。而在大多数情况下我们可以使用生成器来对列表生成式进行进行替代，因为生成器中的元素是按照指定函数推演出来的，只有调用时才生成相应的数据，大大节省了内存空间。  
&emsp;&emsp;这里可以使用一个生成100w个数的列表和生成器进行对比:
```python
import time
import sys
 
time_start = time.time()
g1 = [x for x in range(10000000)]
time_end = time.time()
print('列表生成式返回结果花费的时间： %s' % (time_end - time_start))
print('列表生成式返回结果占用内存大小：%s' % sys.getsizeof(g1))
 
time_start = time.time()
g2 = (x for x in range(10000000))
time_end = time.time()
print('生成器返回结果花费的时间： %s' % (time_end - time_start))
print('生成器返回结果占用内存大小：%s' % sys.getsizeof(g2))

output:
    列表生成式返回结果花费的时间： 0.8428354263305664
    列表生成式返回结果占用内存大小：81528056
    生成器返回结果花费的时间： 0.00023865699768066406
    生成器返回结果占用内存大小：120
```
&emsp;&emsp;可见，生成器返回结果的时间几乎为0，结果所占内存空间的大小相对于列表生成器来说也要小的多。

### 8. 使用生成器替代直接返回列表的函数
&emsp;&emsp;使用生成器替代直接返回列表的函数不仅可以降低内存占用而且可以使代码更加简洁。原理与上面使用生成器替代列表生成器类似。
```python
import sys

def generate_list(x):
    result = []
    for i in range(x):
        result.append(i)
    return result

def genertor(x):
    for i in range(x):
        yield i

l = generate_list(10000)
g = genertor(10000)

print("使用返回列表的函内存占用:{}".format(sys.getsizeof(l)))
print("使用生成器内存占用:{}".format(sys.getsizeof(g)))

output:
    使用返回列表的函内存占用:87624
    使用生成器内存占用:120
```




##### 参考文献
- 《编写高质量python代码的59个有效方法》
- https://www.jianshu.com/p/c5582e23b26c



# Advanced_Python
Python一些比较高阶的使用技巧

#### 1. Pythonic  
&emsp;&emsp;对于想要python进阶的朋友首先由要学习的就是代码风格，一手漂亮的代码是人们识别初级程序员与高级程序员最简单直接的方式。Pythonic是由Google在[《python代码风格指南》](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)中提出的，其中包含了类、函数、注释、TODO等各种python常用的功能所提倡的书写方式。需要进行学习的朋友可以直接点击[这里](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)查看完整的指南。

#### 2.可变对象与不可变对象
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

<img src="https://raw.githubusercontent.com/AnchoretY/images/master/blog/image.57cmchxkny8.png" alt="image" style="zoom:10%;" />

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


#### 3. 参数传递
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

#### 4. 字符串拼接
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
#### 5.生成器、迭代器  

#### 6. raise  

#### 7. 装饰器


##### 参考文献
- https://www.jianshu.com/p/c5582e23b26c


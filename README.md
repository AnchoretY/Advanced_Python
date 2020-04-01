# Advanced_Python
Python一些比较高阶的使用技巧

#### 1. Pythonic  
&emsp;&emsp;对于想要python进阶的朋友首先由要学习的就是代码风格，一手漂亮的代码是人们识别初级程序员与高级程序员最简单直接的方式。Pythonic是由Google在[《python代码风格指南》](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)中提出的，其中包含了类、函数、注释、TODO等各种python常用的功能所提倡的书写方式。需要进行学习的朋友可以直接点击[这里](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)查看完整的指南。

#### 2.可变对象与不可变对象
&emsp;&emsp;

#### 3. 字符串拼接
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
#### 3.生成器、迭代器  

#### 3. raise  

#### 4. 装饰器





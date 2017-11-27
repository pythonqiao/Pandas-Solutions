#### `DataFrame`是由类型可能不同的列组成的二维标签化数据结构。

> 可以想象成一个电子表格，或一个SQL数据表，或是一个由Series对象组成的字典！

基本构造方法：

```py
>>>pd.DataFrame(data, index, columns)   #index, columns非必选
```

---

**一旦给定了index或columns参数，即确定了最终结果**`DataFrame`**的index或columns。因此，若data中包含了原始的index或columns信息，则与提供的index或columns参数相匹配的数据会保留，未匹配的会直接丢弃掉，而结果**`DataFrame`**的缺失值以**`NaN`**代替。**

> 可以理解为以提供的index或columns参数为主体，与data参数中的数据进行类似`left join`运算，匹配到的有数据，未匹配到的用`NaN`值代替

---

#### data参数常见情况如下：

* ##### 嵌套字典 或 Series构成的字典

  * 对于嵌套字典，内层字典会首先被转换成`Series`对象，变成“Series构成的字典”；

  * 对于由Series构成的字典，**字典的键将用作`DataFrame`的columns**，进而与提供的columns参数匹配（若提供的话）；

  * 字典中各个Series的index将取**并集**，进而与提供的index参数匹配（若提供的话）；

  ```py
  >>>d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
     ....:'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
  >>>pd.DataFrame(d) # index取并集
     one  two
  a  1     1
  b  2     2
  c  3     3
  d  NaN   4
  >>>pd.DataFrame(d, index=['d', 'b', 'a'], columns=['two', 'three']) # 传入的columns会覆盖原字典的键
     two three
  d   4   NaN
  b   2   NaN
  a   1   NaN
  ```
* ##### 列表或数组构成的字典

  * 列表或数组的长度必须一致，否则会引起`ValueError`；若提供索引，则索引的长度也必须和列表或数组的长度一致。

  * **字典的键将用作`DataFrame`的columns。**

  ```py
  >>>d = {'one' : [1., 2., 3., 4.],
     ....:'two' : [4., 3., 2., 1.]}
  >>>pd.DataFrame(d, index=['a', 'b', 'c', 'd']) # 不提供index的话，则默认为整型索引
     one  two
  a  1.0  4.0
  b  2.0  3.0
  c  3.0  2.0
  d  4.0  1.0
  ```

  * 相关的构造函数有**DataFrame.from\_dict**和**DataFrame.from\_items。**`DataFrame.from_dict`需要的参数为嵌套字典或数组类型的序列组成的字典，`DataFrame.from_items`需要的参数为`(key, value)`键值对组成的序列。

    默认情况下，`DataFrame.from_dict`将外层字典的键作为结果的columns，`DataFrame.from_items`将键值对的key作为结果的columns；当把`orient`参数设置为`index`时，上述情况将作为结果的index。

  ```py
  >>>pd.DataFrame.from_items([('A', [1, 2, 3]), ('B', [4, 5, 6])])
     A  B
  0  1  4
  1  2  5
  2  3  6

  >>>pd.DataFrame.from_items([('A', [1, 2, 3]), ('B', [4, 5, 6])],
     ....:                   orient='index', columns=['one', 'two', 'three']) # 设置orient参数
      one  two  three
  A    1    2      3
  B    4    5      6
  ```

* ##### 字典构成的列表

  * 列表中的**每个字典都将变为结果`DataFrame`中的一行数据**；

  * 每个字典中的键取并集，用作结果`DataFrame`的columns，数据缺失时用`NaN`代替；

  * 默认为整型索引，若提供index，则其长度应与列表长度一致（即等于列表中的字典数）；

  ```py
  >>>data2 = [{'a': 1, 'b': 2}, {'a': 5, 'b': 10, 'c': 20}]
  >>>pd.DataFrame(data2)
     a   b     c
  0  1   2   NaN
  1  5  10  20.0
  >>>pd.DataFrame(data2, index=['first', 'second'], columns=['a', 'b'])
          a   b
  first   1   2
  second  5  10
  ```
* ##### _元组构成的嵌套字典（用于生成层次化索引的DataFrame）_

  * 类似普通嵌套字典，外层字典的键用作结果的columns，内层字典的键用作结果的index，也可理解为先将内层字典转换为一个`Series`；

  ```
  >>>pd.DataFrame({('a', 'b'): {('A', 'B'): 1, ('A', 'C'): 2},
     ....:         ('a', 'a'): {('A', 'C'): 3, ('A', 'B'): 4},
     ....:         ('a', 'c'): {('A', 'B'): 5, ('A', 'C'): 6},
     ....:         ('b', 'a'): {('A', 'C'): 7, ('A', 'B'): 8},
     ....:         ('b', 'b'): {('A', 'D'): 9, ('A', 'B'): 10}})

         a              b      
         a    b    c    a     b
  A B  4.0  1.0  5.0  8.0  10.0
    C  3.0  2.0  6.0  7.0   NaN
    D  NaN  NaN  NaN  NaN   9.0
  ```
* ##### _一个Series_

  * 此情况下生成的`DataFrame`，与原`Series`保持相同的index，并且只有一列数据，列名columns与原Series相同，除非重新指定新列名。

  ```py
  >>>a = pd.Series([1, 2, 3])
  >>>a
  0    1
  1    2
  2    3
  dtype: int64
  >>>pd.DataFrame(a)
     0
  0  1
  1  2
  2  3
  ```




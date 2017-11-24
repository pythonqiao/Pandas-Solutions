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

  * 对于嵌套字典，内层字典会首先被转换成`Series`对象，变成Series构成的字典；

  * 对于由Series构成的字典，字典的键将用作`DataFrame`的columns，进而与提供的columns参数匹配（若提供的话）；

  * 字典中各个Series的index将取**并集**，进而与提供的index参数匹配（若提供的话）；




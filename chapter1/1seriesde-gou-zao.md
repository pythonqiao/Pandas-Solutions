导入`numpy`和`pandas`模块：

```py
>>>import numpy as np
>>>import pandas as pd
```

创建Series对象的基本方法如下：

```py
>>>s = pd.Series(data, index=index)
```

其中，data参数的类型有以下三种情况：

* #### 一维数组

  * 这种情况下，可不提供index参数，此时自动生成整型索引\[0, ..., len\(data\) - 1\]；

    ```py
    >>>pd.Series(np.random.randn(5))
    0   -0.1732
    1    0.1192
    2   -1.0442
    3   -0.8618
    4   -2.1046
    dtype: float64
    ```

  * 若提供index参数（序列类型），则index的长度必须和data数组的长度一致！

    ```py
    >>>pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
    a    0.4691
    b   -0.2829
    c   -1.5091
    d   -1.1356
    e    1.2121
    dtype: float64
    ```
* #### 字典

  * 不提供index参数，则将以字典的键和值分别作为Series的索引和对应的值：

    ```py
    >>>d = {'a' : 0., 'b' : 1., 'c' : 2.}
    >>>pd.Series(d)
    a    0.0
    b    1.0
    c    2.0
    dtype: float64
    ```

  * 提供index参数，则会对index的各个值与字典d的各个键进行匹配，匹配一致的，正常显示字典键对应的值，匹配失败的，以`NaN`值替代：

    > `NaN`是pandas中缺失值的标准标记形式，不是一个数

    ```py
    >>>pd.Series(d, index=['b', 'c', 'd', 'a'])
    b    1.0
    c    2.0
    d    NaN
    a    0.0
    dtype: float64
    ```
* #### 标量值

  * 这种情况下，必须提供index参数，结果Series中，该标量值重复次数与index的长度一致：

    ```py
    >>>pd.Series(5, index=['a', 'b', 'c', 'd', 'e'])
    a    5
    b    5
    c    5
    d    5
    e    5
    dtype: int64
    ```




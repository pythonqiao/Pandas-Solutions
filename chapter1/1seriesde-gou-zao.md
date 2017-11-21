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

* ##### 一维数组

    （1）这种情况下，可不提供index参数，此时自动生成整型索引\[0, ..., len\(data\) - 1；
* ```py
  >>>pd.Series(np.random.randn(5))
  0   -0.1732
  1    0.1192
  2   -1.0442
  3   -0.8618
  4   -2.1046
  dtype: float64
  ```
    （2）若提供index参数（序列类型），则index的长度必须和data数组的长度一致！
* ```py
  >>>pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
  a    0.4691
  b   -0.2829
  c   -1.5091
  d   -1.1356
  e    1.2121
  dtype: float64
  ```


##### 




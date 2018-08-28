## 背景
Python 官网：https://www.python.org/  
Python Modules 文档：https://docs.python.org/3/tutorial/modules.html  
Python 相关版本：v3.7.0  

## 正文

### 1. module
  
Python模块就是一个`.py`文件，其中包含了定义和语句。  
该模块的名字，就是文件名。  
  
除了函数定义之外，Python模块中还可以包含可以执行的语句，  
该模块只有在第一次被导入的时候，这些可执行语句才被执行，且只执行一次。  
  
此外，每一个模块都有自己的私有符号表，  
因此，模块的作者可以放心的使用全局变量，而不用担心命名冲突。  
  
例如，我们定义以下两个模块，  
```
# ./import-module/m1.py

a = 1
```
  
```
# ./import-module/dir/m2.py

b = 2
```
  
然后在主程序中导入它们，  
```
# ./import-module/main.py

import m1
import dir.m2

print(m1.a)
print(dir.m2.b)
```
  
执行一下，  
```
$ python3 main.py
1
2
```
  
### 2. package
  
参考文档：https://docs.python.org/3/tutorial/modules.html#packages  
  
Python中的package可用于管理多个module的命名空间。  
例如，`A.B`表示，package `A`中的子模块`B`。  
  
一个典型的package，目录结构如下，  
```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```
  
其中`__init__.py`文件，使得Python可以将整个目录看做一个package。  
package的用户，也可以直接使用package内的子模块，例如，  
```
import sound.effects.echo
```
  
同时也必须使用全名来调用它，例如，  
```
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```
  
**注：**  
（1）使用`from ... import ...`，简化名字前缀，  
```
from sound.effects import echo
echo.echofilter(input, output, delay=0.7, atten=4)
```
  
```
from sound.effects.echo import echofilter
echofilter(input, output, delay=0.7, atten=4)
```
  
（2）`from package import item`，  
其中，`item`或者是`package`中的一个子模块/子包，  
或者是`package`中定义的一个名字。  
  
`import`语句，首先会检测`item`是否在`package`中被定义了，  
如果没有被定义，就假定`item`是一个模块去加载它，  
如果没有找到这个模块，就会抛ImportError异常。  
  
### 3. import *
  
参考文档：https://docs.python.org/3/tutorial/modules.html#importing-from-a-package  

package根目录下的`__init__.py`文件中，可以设置`__all__`变量，  
它会影响该package被`from package import *`时，所导入的**模块**列表。  
  
例如，我们创建了一个名为`pkg`的包，它只导出`m1`模块，  
```
# ./import-all/pkg/m1.py

x = 1
```
```
# ./import-all/pkg/m2.py

y = 1
```
```
# ./import-all/pkg/__init__.py

__all__ = ["m1"]
```
  
主程序，  
```
# ./import-all/main.py

from pkg import *

print(m1.x)  # 1
print(m2.y)  # NameError: name 'm2' is not defined
```
  
**注：**  
（1）`import xxx`只能导入**模块**，不能控制导入的变量。  
  
（2）`import package.module`会导致`package`，`package.module`两个名字都被导入。  
  
（3）`from A import B`， 可以导入模块或者变量。  
`from module import variable`，会导入一个变量`variable = module.variable`，但是`module`这个名字没有导入。  
`from package import module`，会导入一个变量`module = package.module`， 但是`package`这个名字没有导入。  
  
（4）`from package import *`，对于`import *`导入来说，  
从`package`导入的**模块**列表，是受`package/__init__.py`中`__all__`变量控制的。  

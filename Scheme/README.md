## 背景
Scheme 官网：http://www.r6rs.org/  
Scheme Libraries 文档：http://www.r6rs.org/final/html/r6rs/r6rs-Z-H-10.html#node_chap_7  
Scheme 相关版本：R6RS  

## 正文

### 1. Mac OS X 上安装 Chez Scheme
Chez Scheme以前是个人项目，现在开源了，由Cisco公司维护，  
[cisco/ChezScheme](https://github.com/cisco/ChezScheme)  
  
#### 1.1 下载最新的Release版
Github Release页面，[ChezScheme/releases](https://github.com/cisco/ChezScheme/releases)，  
我们下载最新的版本，[csv9.5.tar.gz](https://github.com/cisco/ChezScheme/releases/download/v9.5/csv9.5.tar.gz)。  
  
#### 1.2 安装
双击下载后的文件`csv9.5.tar.gz`，就会自动解压缩。  
  
进入解压缩后的文件夹中，执行以下命令，  
```
$ ./configure
$ sudo make install
```
  
安装完成后，会自动将`scheme`和`scheme-script`加入到环境变量中，我们可以用以下方式打开REPL，  
```
$ scheme
Chez Scheme Version 9.5
Copyright 1984-2017 Cisco Systems, Inc.

>
```
  
#### 1.3 解释执行
给`scheme-script`命令传入一个入口文件地址，就可以解释执行了，  
```
$ scheme-script main.ss
> hello
```
  
**注：**  
我在执行`make install`的过程中，报错了，  
```
fatal error: 'X11/Xlib.h' file not found
#include <X11/Xlib.h>
         ^
1 error generated.
make[2]: *** [expeditor.o] Error 1
make[1]: *** [build] Error 2
make: *** [install] Error 2
```
  
解决办法是，下载和安装[XQuartz](https://www.xquartz.org/)  

### 2. top-level programs
参考文档：https://www.scheme.com/tspl4/libraries.html#g145  
  
top-level programs是直接可以作为`scheme-script`入口的程序文件，  
```
; ./top-level-programs/main.ss

(import (rnrs))

(display "hello")
```
在top-level programs中，我们使用`import`导入了`rnrs`内置模块，  
然后输出"hello"。  
  
### 3. library
参考文档：https://www.scheme.com/tspl4/libraries.html#g144  
  
#### 3.1 定义library
```
; ./library/tool/lib.ss

(library (tool lib (1))
    (export x)
    (import (rnrs))
    (define x 3))
```
我们使用以上方式，在目录`./tool/`中定义了一个模块`lib`，  
它的版本号是`1`。  
  
它使用`export`导出了变量了`x`，  
使用`import`导入了标准库`rnrs`。  
  
更多标准库：https://www.scheme.com/tspl4/libraries.html#g143  
  
#### 3.2 使用library
```
; ./library/main.ss

(import (rnrs) (tool lib))

(display x)
```
我们使用`import`导入了模块`(tool lib)`。  
  
**注：**  
（1）import会根据top-level programs所在目录，查找文件或者目录中的文件。  
`(import (tool lib))`会在`./main.ss`所在目录，查找`tool/lib.ss`文件。  
  
（2）library定义的模块名，也应该与文件路径一致。  
`(library (tool lib))`其中`tool`是文件夹名，`lib`是文件名。  
  
（3）如果在`tool/lib.ss`中引用`tool/another-lib.ss`，  
`import`模块的写法与top-level programs一样，也要从`tool`开始指定模块名，例如`(import (tool another-lib)`，  
不能写相对路径`(import (another-lib)`。  

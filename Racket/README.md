## 背景
Racket 官网：https://racket-lang.org/  
Racket Modules 文档：https://docs.racket-lang.org/guide/modules.html  
Racket 相关版本：v7.0  

## 正文

### 1. 命令行工具 raco
参考文档：https://docs.racket-lang.org/raco/  
  
实际上，[下载](https://download.racket-lang.org/)Racket并安装，  
就已经内置`raco`命令行工具了。  
  
在Mac OS X上面，`raco`位于`/Applications/Racket v7.0/bin/`目录下。  
要想在命令行中使用 `raco`，则需要将该目录加入到`$PATH`环境变量中。  
  
例如，我使用了[Oh My ZSH](https://ohmyz.sh/)，  
于是需要修改`~/.zshrc`文件，在其末尾加入，  
```
export PATH="/Applications/Racket v7.0/bin/":$PATH
```
  
然后在shell中使用`source`命令，加载并执行`~/.zshrc`即可，  
```
$ source ~/.zshrc
```
  
最后我们执行`raco`看看效果，  
```
$ raco

Usage: raco <command> <option> ... <arg> ...

Frequently used commands:
  docs                 search and view documentation
  make                 compile source to bytecode
  setup                install and build libraries and documentation
  pkg                  manage packages
  planet               manage Planet package installations
  exe                  create executable
  test                 run tests associated with files/directories

A command can be specified by an unambiguous prefix.
See `raco help' for a complete list of commands.
See `raco help <command>' for help on a command.
```

### 2. 解释执行
  
我们新建一个`main.rkt`文件，  
```
; ./interpret/main.rkt

#lang racket

(println (+ 1 2))
```
  
调用`racket`解释执行，  
```
$ racket main.rkt
3
```
  
### 3. 编译执行
  
`raco exe`可以将源码进行编译，并与racket运行时进行打包，  
得到一个完整的可执行文件。  
  
```
$ raco exe main.rkt
```  
以上命令，会在`main.rkt`同级目录中，生成一个名为`main`的可执行文件。  
  
```
./compile
├── main
└── main.rkt
```
  
运行一下，  
```
$ ./main
3
```
  
### 4. 模块的导出和导入
  
#### 4.1 模块的导出
在Racket中，一个模块通常就是一个文件，  
以下我们新建了一个`cake.rkt`文件，它作为一个模块导出了`print-cake`函数，  
```
; ./export-and-import/cake.rkt

#lang racket
 
(provide print-cake)
 
; draws a cake with n candles
(define (print-cake n)
  (show "   ~a   " n #\.)
  (show " .-~a-. " n #\|)
  (show " | ~a | " n #\space)
  (show "---~a---" n #\-))
 
(define (show fmt n ch)
  (printf fmt (make-string n ch))
  (newline))
```
其中，`provide`指定了模块导出的变量。  
  
#### 4.2 模块的导入
  
其他文件中，可以使用`require`导入模块，  
```
; ./export-and-import/main.rkt

#lang racket
 
(require "cake.rkt")
 
(print-cake (random 30))
```
  
执行一下，  
```
$ racket ./main.rkt

   ...........................
 .-|||||||||||||||||||||||||||-.
 |                             |
---------------------------------
```
  
其中，`(require "cake.rkt")`也可以写为`(require "./cake.rkt")`，  
指的是与`./main.rkt`的相对路径。  
  
**注：**  
（1）Racket还提供了另外一种称为module syntax的语法来定义模块，  
可以参考这里：https://docs.racket-lang.org/guide/Module_Syntax.html  
  
（2）DrRacket分为上下两块，上面的称为定义区（definitions area），下面的称为交互区（interactions area）。  
在定义区，我们可以使用绝对路径来导入模块，  
```
; ./absolute-path/add1.rkt

#lang racket
(require (file "~/Github/awesome-modules/Racket/examples/absolute-path/add1.rkt"))
```
  
运行定义区的代码之后，在交互区，就可以直接使用导入的函数了，  
```
> (add1 2)
3
```

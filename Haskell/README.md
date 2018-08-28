## 背景
Haskell 官网：https://www.haskell.org/  
Haskell Modules 文档：https://www.haskell.org/onlinereport/haskell2010/haskellch5.html#x11-980005  
Haskell 相关版本：2010  

## 正文

### 1. module name
一个模块名是一系列用点号分隔的标识符。  
这些标识符首字母都是大写，点号两边没有空格。  

> A module name is a sequence of one or more identifiers beginning with capital letters, separated by dots, with no intervening spaces.

一个`qualified name`由`模块名.unqualifed name`构成。  

### 2. import declarations
（1）`import A(f)`：导入值（value），字段名（field name），类方法（class method），其中`A`是被导入的模块名  
（2）`import A(T)`：只导入类型构造器（type constructor）/ 只导入类型类（class）  
（3）`import A(T(f,g))`：导入类型构造器和给定的值构造器（data constructor）和字段名（field name）/ 只导入类型类和给定的类方法（class method）  
（4）`import A(T(..))`：导入类型构造器和它所有的值构造器，和所有的字段名 / 导入类型类和它所有的类方法  
（5）`import A()`：不导入模块`A`中的任何entity  
（6）`import A`：导入模块`A`中所有的entity  
（7）`import qualifed A`：导入A中的qualified name（例：只导入`A.f`）  
（8）`import qualifed A as B`：导入A中的entity，并修改qualified name的模块名。（例：`B.f`）  

**注：**  
（1）默认所有导入方式，都导入instance declarations  
（2）以上`T`不可以是值构造器，值构造器只能放在`T`的括号中导入  
（3）值构造器可以直接写在`hiding`中，此时屏蔽同名的值构造器，类型构造器以及类型类  
（4）如果不使用`qualified`导入方式，则`qualifed name`和`unqualified name`都被导入，如果使用了`qualified`导入方式，则只导入`qualifed name`。  

> If the import declaration used the qualified keyword, only the qualified name of the entity is brought into scope. If the qualified keyword is omitted, then both the qualified and unqualified name of the entity is brought into scope.

### 3. export list
（1）`module A(f) where`：同import declarations  
（2）`module A(T) where`：同import declarations  
（3）`module A(T(f,g)) where`：同import declarations  
（4）`module A(T(..)) where`：同import declarations  
（5）`module A(module B) where`：导出模块`B`中所有的entity  
（6）`module A(module A) where`：导出本模块中所有的entity  
（7）`module A() where`：不导出任何entity  
（8）`module A where`：导出所有的entity  

**注：**  
（1）默认所有导出方式，都导出instance declarations  
（2）以上`T`不可以是值构造器，值构造器只能放在`T`的括号中导出  
（3）`module A where`只能导出模块中所有的值，类型和类型类，但是不能导出模块导入的名字。  

> A module implementation may only export an entity that it declares, or that it imports from some other module. If the export list is omitted, all values, types and classes defined in the module are exported, but not those that are imported.

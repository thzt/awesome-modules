## 背景
Node.js 官网：https://nodejs.org/en/  
Node.js Modules 文档：https://nodejs.org/api/modules.html  
Node.js 相关版本：v10.9.0  

## 正文

### 1. 模块的构成
在Node.js中，每一个文件都被看做单独的模块，  
例如，将以下代码放到`./circle.js`中，就构成了一个`circle.js`模块。  

```
// ./exports-and-require/circle.js

const { PI } = Math;

exports.area = (r) => PI * r ** 2;
exports.circumference = (r) => 2 * PI * r;
```
通过对内置对象`exports`对象添加属性，  
该模块导出了一个对象，`{area, circumference}`，  
它有两个方法，`area`和`circumference`。  

### 2. 模块的导入
其他文件中，就可以使用`require`关键字，  
根据`circle.js`模块的相对路径，来导入`circle.js`模块了。  
```
// ./exports-and-require/foo.js

const circle = require('./circle.js');
console.log(`The area of a circle of radius 4 is ${circle.area(4)}`);
```

`require`会将`circle.js`模块中导出的对象`{area, circumference}`，  
导入为`circle`变量。  

因此，`circle.area`就是`circle.js`模块中定义的`area`函数了。  

### 3. 模块内的私有变量
没有被模块显式导出的名字，是模块内的私有变量。  
例如，`circle.js`模块中的`PI`，在模块外是无法访问的。  

### 4. 默认导出
以上示例中，我们通过对内置对象`exports`添加属性，  
指定了模块的导出对象，`{area, circumference}`。  

那么，我们可否不导出一个对象，而只导出一个函数呢？  
是可以的。  

Node.js中，通过对`module.exports`重新赋值，可以强行指定模块的导出物。例如，  
```
// ./module.exports/square.js

module.exports = class Square {
  constructor(width) {
    this.width = width;
  }

  area() {
    return this.width ** 2;
  }
};
```
以上模块只导出了一个`Square`类。  

在进行模块导入的时候，该类会被直接导入到相应的变量中。  
```
// ./module.exports/bar.js

const Square = require('./square.js');

const mySquare = new Square(2);
console.log(`The area of mySquare is ${mySquare.area()}`);
```
其中，变量`Square`就是`square.js`中导出的那个类了。  

- - -

## 其他

Node.js modules 遵循了 [CommonJS 规范](http://www.commonjs.org/)，  
这是相关的 [wiki](http://wiki.commonjs.org/wiki/CommonJS)。  

# JavaScript-Module
JavaScript模块


JavaScript模块

背景：JavaScript规模和复杂性越来越高，但是JavaScript原本不是模块化编程语言，不支持类（class）。ES6支持“类”和“模块”。

模块不同写法：

1、原始写法：将代码放一起也算是模块（污染全局变量，模块间关系不明确）

```javascript
function f1(){...};
function f2(){...};
```

2、对象写法：将成员集中在对象里（所有成员暴露，并可改写）

```javascript
var module = new Object({
	_c = 0,
	m1 : function(){...},
	m2 : function(){...},
});
```

3、立即执行函数写法：原始模块写法（不暴露成员）

```javascript
var module = (function(){
    var _c = 0;
    var m1 = function(){...};
    var m2 = function(){...};
    return {
        m1 : m1,
        m1 : m1,
    };
});
```

4、放大模式（涉及多个模块及引用）

```javascript
var module = (function(mod){
    mod.m3 = function(){...};
    return mod;
})(module);
```

5、宽放大模式（对比放大模式区别在参数）

```javascript
var module = (function(mod){
    mod.m3 = function(){...};
    return mod;
})(window.module || {});
```

6、全局变量参数（模块具有独立性）

```javascript
var module = (function ($, YAHOO) {...})(jQuery, YAHOO);
```

7、模块规范分CommonJS和AMD

8、CommonJS

NodeJS诞生于JavaScript服务端编程。基于CommonJS规范，使用require()全局方法引入模块。

```javascript
var math = require('math');
math.add(2,3); // 5
```

9、浏览器的模块化

服务器资源存储在本地，获取资源不同于浏览器的网络请求资源，而是从本地读取。所以不存在浏览器的问题。

CommonJS不适用于浏览器环境，1、同步执行，导致浏览器等待而假死；由此诞生AMD（异步模块定义）。

10、AMD（引入模块后，异步执行后续代码）

```javascript
require([module], callback);
e.g.
require(['math'], function (math) {
	math.add(2, 3);
});
```

AMD规范库：require.js、curl.js

11、require.js

主要功能：1、异步加载js脚本；2、管理模块间依赖；

加载一个模块，浏览器即为发起一次请求，require.js有相关合并优化工具，减少HTTP请求。

加载方式：

```javascript
<script src="js/require.js"></script> // 将脚本放在网页底部，防止页面因加载脚本失去响应
<script src="js/require.js" defer async="true" ></script>
<script src="js/require.js" data-main="js/main"></script> // 加载require.js后，执行main主模块
```

```javascript
require.config({
	baseUrl: "js/lib",
    paths: {
        // "jquery": "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min",
        "underscore": "underscore.min",
        "backbone": "backbone.min"
    }
});
```



```javascript
// 规范加载
define(name, [], callback);
// 非规范加载
require.config({
    shim: {
        'underscore':{
            exports: '_'
        },
        'backbone': {
            deps: ['underscore', 'jquery'], // 该引进模块的依赖
            exports: 'Backbone' // 模块名，必填项
        }
    }
});
```

```javascript
// 插件
require(['domready!'], function (doc){
	// called once the DOM is ready
});
define([
    'text!review.txt',
    'image!cat.jpg'
],
function(review,cat){
    console.log(review);
    document.body.appendChild(cat);
});
// 其他插件json、mdown，用于加载json文件和markdown文件
```


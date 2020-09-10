# HTML

# CSS 

## 基础操作

### 居中与垂直居中：

**行内或类行内元素：**

text-align:center

**块级元素：**

margin :auto

**通用方法：父元素操作**

display:flex justify-content:center





# JS

## JS模块化

### commonjs

:Node.js是commonJS规范的主要实践者，它有四个重要的环境变量为模块化的实现提供支持：**module、exports、require、global**。实际使用时，用module.exports定义当前模块对外输出的接口（不推荐直接用exports），用require加载模块。

### AMD&require.js

AMD规范采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。这里介绍用require.js实现AMD规范的模块化：**用require.config()指定引用路径等，用define()定义模块，用require()加载模块**。
　　首先我们需要引入require.js文件和一个入口文件main.js。main.js中配置require.config()并规定项目中用到的基础模块。

```javascript
script src="js/require.js" data-main="js/main"></script>
 
/** main.js 入口文件/主模块 **/
//首先用config()指定各模块路径和引用名
require.config({
    baseUrl: "js/lib",
    paths: {
        "jquery": "jquery.min",  //实际路径为js/lib/jquery.min.js
        "underscore": "underscore.min",
    }
});
//执行基本操作
require(["jquery","underscore"],function($,_){
    // some code here
});
```

### CMD 

CMD是另一种js模块化方案，它与AMD很类似，不同点在于：AMD 推崇依赖前置、提前执行，CMD推崇依赖就近、延迟执行。此规范其实是在sea.js推广过程中产生的。

```javascript
/** AMD写法 **/
define(["a", "b", "c", "d", "e", "f"], function(a, b, c, d, e, f) {
     // 等于在最前面声明并初始化了要用到的所有模块
    a.doSomething();
    if (false) {
        // 即便没用到某个模块 b，但 b 还是提前执行了
        b.doSomething()
    }
});
 
/** CMD写法 **/
define(function(require, exports, module) {
    var a = require('./a'); //在需要时申明
    a.doSomething();
    if (false) {
        var b = require('./b');
        b.doSomething();
    }
});
 
/** sea.js **/
// 定义模块 math.js
define(function(require, exports, module) {
    var $ = require('jquery.js');
    var add = function(a,b){
        return a+b;
    }
    exports.add = add;
});
// 加载模块
seajs.use(['math.js'], function(math){
    var sum = math.add(1+2);
});
```



### ES6

```javascript
/** 定义模块 math.js **/
var basicNum = 0;
var add = function (a, b) {
    return a + b;
};
export { basicNum, add };
 
/** 引用模块 **/
import { basicNum, add } from './math';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}

/* export default*/
export default { basicNum, add };
//引入
import math from './math';
function test(ele) {
    ele.textContent = math.add(99 + math.basicNum);
}
```

# 一百题

### 28 cookie 和 token 都存放在 header 中，为什么不会劫持 token？

1、首先token不是防止XSS的，而是为了防止CSRF的；
2、CSRF攻击的原因是浏览器会自动带上cookie，而浏览器不会自动带上token

### 29 聊聊 Vue 的双向数据绑定，Model 如何改变 View，View 又是如何改变 Model 的

Model 改变 View的过程： 依赖于ES5的object.defindeProperty，通过 defineProperty 实现的数据劫持，getter 收集依赖，setter 调用更新回调（不同于观察者模式，是发布订阅 + 中介）
View 改变 Model的过程： 依赖于 v-model ,该语法糖实现是在单向数据绑定的基础上，增加事件监听并赋值给对应的Model

附带补一个问题， 如果react也要双向绑定（虽然推荐是单向的），其实早期mixin【React.addons.LinkedStateMixin】可以实现, 也就是搞一些简单的setstate封装吧

### 30 两个数组合并成一个数组

```js
const arr1 = ['A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'D1', 'D2']
const arr2 = ['A', 'B', 'C', 'D']

function merge (arr1, arr2) {
    let [i, j] = [0, 0];
    const result = [];
    while(i<arr1.length && j<arr2.length) {
        if(arr1[i].charAt(0) === arr2[j].charAt(0)) {
            result.push(arr1[i++]);
        } else {
            result.push(arr2[j++]);
        }
    }
    while(i<arr1.length) result.push(arr1[i++]);
    while(j<arr2.length) result.push(arr2[j++]);
  	// 一种合并的好方法。
  	return [...res, ...a.slice(left), ...b.slice(right)]
    //return result;
}
console.log(merge(arr1, arr2));
```

### 第 31 题：改造下面的代码，使之输出0 - 9，写出你能想到的所有解法。

```js
for (var i = 0; i< 10; i++){
	setTimeout(() => {
		console.log(i);
    }, 1000)
}
```

解答：

```js
for (let i = 0; i< 10; i++){
    setTimeout((i) => {
    console.log(i);
    }, 1000,i)
 }
// 让传入的参数为 0 - 9
for (var i = 0; i< 10; i++){
    (i => {
        setTimeout(() => {
            console.log(i);
        }, 1000)
    })(i)
}
```

### 第 32 题：Virtual DOM 真的比操作原生 DOM 快吗？谈谈你的想法。

作者：尤雨溪
链接：https://www.zhihu.com/question/31809713/answer/53544875
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



**1. 原生 DOM 操作 vs. 通过框架封装操作。**

这是一个性能 vs. 可维护性的取舍。框架的意义在于为你掩盖底层的 DOM 操作，让你用更声明式的方式来描述你的目的，从而让你的代码更容易维护。没有任何框架可以比纯手动的优化 DOM 操作更快，因为框架的 DOM 操作层需要应对任何上层 API 可能产生的操作，它的实现必须是普适的。针对任何一个 benchmark，我都可以写出比任何框架更快的手动优化，但是那有什么意义呢？在构建一个实际应用的时候，你难道为每一个地方都去做手动优化吗？出于可维护性的考虑，这显然不可能。框架给你的保证是，你在不需要手动优化的情况下，我依然可以给你提供过得去的性能。

**2. 对 React 的 Virtual DOM 的误解。**

React 从来没有说过 “React 比原生操作 DOM 快”。React 的基本思维模式是每次有变动就整个重新渲染整个应用。如果没有 Virtual DOM，简单来想就是直接重置 innerHTML。很多人都没有意识到，在一个大型列表所有数据都变了的情况下，重置 innerHTML 其实是一个还算合理的操作... 真正的问题是在 “全部重新渲染” 的思维模式下，即使只有一行数据变了，它也需要重置整个 innerHTML，这时候显然就有大量的浪费。

我们可以比较一下 innerHTML vs. Virtual DOM 的重绘性能消耗：

- innerHTML:  render html string **O(template size)** + 重新创建所有 DOM 元素 **O(DOM size)**
- Virtual DOM: render Virtual DOM + diff **O(template size)** + 必要的 DOM 更新 **O(DOM change)**



Virtual DOM render + diff 显然比渲染 html 字符串要慢，但是！它依然是纯 js 层面的计算，比起后面的 DOM 操作来说，依然便宜了太多。可以看到，innerHTML 的总计算量不管是 js 计算还是 DOM 操作都是和整个界面的大小相关，但 Virtual DOM 的计算量里面，只有 js 计算和界面大小相关，DOM 操作是和数据的变动量相关的。前面说了，和 DOM 操作比起来，js 计算是极其便宜的。这才是为什么要有 Virtual DOM：它保证了 1）不管你的数据变化多少，每次重绘的性能都可以接受；2) 你依然可以用类似 innerHTML 的思路去写你的应用。

**3. MVVM vs. Virtual DOM**

相比起 React，其他 MVVM 系框架比如 Angular, Knockout 以及 Vue、Avalon 采用的都是数据绑定：通过 Directive/Binding 对象，观察数据变化并保留对实际 DOM 元素的引用，当有数据变化时进行对应的操作。MVVM 的变化检查是数据层面的，而 React 的检查是 DOM 结构层面的。MVVM 的性能也根据变动检测的实现原理有所不同：Angular 的脏检查使得任何变动都有固定的 **O(watcher count)** 的代价；Knockout/Vue/Avalon 都采用了依赖收集，在 js 和 DOM 层面都是 **O(change)**：

- 脏检查：scope digest **O(watcher count)** + 必要 DOM 更新 **O(DOM change)**
- 依赖收集：重新收集依赖 **O(data change)** + 必要 DOM 更新 **O(DOM change)**

可以看到，Angular 最不效率的地方在于任何小变动都有的和 watcher 数量相关的性能代价。但是！当所有数据都变了的时候，Angular 其实并不吃亏。依赖收集在初始化和数据变化的时候都需要重新收集依赖，这个代价在小量更新的时候几乎可以忽略，但在数据量庞大的时候也会产生一定的消耗。

MVVM 渲染列表的时候，由于每一行都有自己的数据作用域，所以通常都是每一行有一个对应的 ViewModel 实例，或者是一个稍微轻量一些的利用原型继承的 "scope" 对象，但也有一定的代价。所以，MVVM 列表渲染的初始化几乎一定比 React 慢，因为创建 ViewModel / scope 实例比起 Virtual DOM 来说要昂贵很多。这里所有 MVVM 实现的一个共同问题就是在列表渲染的数据源变动时，尤其是当数据是全新的对象时，如何有效地复用已经创建的 ViewModel 实例和 DOM 元素。假如没有任何复用方面的优化，由于数据是 “全新” 的，MVVM 实际上需要销毁之前的所有实例，重新创建所有实例，最后再进行一次渲染！这就是为什么题目里链接的 angular/knockout 实现都相对比较慢。相比之下，React 的变动检查由于是 DOM 结构层面的，即使是全新的数据，只要最后渲染结果没变，那么就不需要做无用功。

Angular 和 Vue 都提供了列表重绘的优化机制，也就是 “提示” 框架如何有效地复用实例和 DOM 元素。比如数据库里的同一个对象，在两次前端 API 调用里面会成为不同的对象，但是它们依然有一样的 uid。这时候你就可以提示 track by uid 来让 Angular 知道，这两个对象其实是同一份数据。那么原来这份数据对应的实例和 DOM 元素都可以复用，只需要更新变动了的部分。或者，你也可以直接 track by $index 来进行 “原地复用”：直接根据在数组里的位置进行复用。在题目给出的例子里，如果 angular 实现加上 track by $index 的话，后续重绘是不会比 React 慢多少的。甚至在 dbmonster 测试中，Angular 和 Vue 用了 track by $index 以后都比 React 快: [dbmon](https://link.zhihu.com/?target=http%3A//vuejs.github.io/js-repaint-perfs/) (注意 Angular 默认版本无优化，优化过的在下面）

顺道说一句，React 渲染列表的时候也需要提供 key 这个特殊 prop，本质上和 track-by 是一回事。

**4. 性能比较也要看场合**

在比较性能的时候，要分清楚初始渲染、小量数据更新、大量数据更新这些不同的场合。Virtual DOM、脏检查 MVVM、数据收集 MVVM 在不同场合各有不同的表现和不同的优化需求。Virtual DOM 为了提升小量数据更新时的性能，也需要针对性的优化，比如 shouldComponentUpdate 或是 immutable data。

- 初始渲染：Virtual DOM > 脏检查 >= 依赖收集
- 小量数据更新：依赖收集 >> Virtual DOM + 优化 > 脏检查（无法优化） > Virtual DOM 无优化
- 大量数据更新：脏检查 + 优化 >= 依赖收集 + 优化 > Virtual DOM（无法/无需优化）>> MVVM 无优化

不要天真地以为 Virtual DOM 就是快，diff 不是免费的，batching 么 MVVM 也能做，而且最终 patch 的时候还不是要用原生 API。在我看来 Virtual DOM 真正的价值从来都不是性能，而是它 1) 为函数式的 UI 编程方式打开了大门；2) 可以渲染到 DOM 以外的 backend，比如 ReactNative。

**5. 总结**

以上这些比较，更多的是对于框架开发研究者提供一些参考。主流的框架 + 合理的优化，足以应对绝大部分应用的性能需求。如果是对性能有极致需求的特殊情况，其实应该牺牲一些可维护性采取手动优化：比如 Atom 编辑器在文件渲染的实现上放弃了 React 而采用了自己实现的 tile-based rendering；又比如在移动端需要 DOM-pooling 的虚拟滚动，不需要考虑顺序变化，可以绕过框架的内置实现自己搞一个。


>ref: https://www.zhihu.com/question/31809713/answer/53544875

### 第 33 题：下面的代码打印什么内容，为什么？

```js
var b = 10;
(function b(){
    b = 20;
    console.log(b); 
})();
```

答案：

 [Function: b]



我的理解是，b函数是一个相当于用const定义的常量，内部无法进行重新赋值，如果在严格模式下，会报错"Uncaught TypeError: Assignment to constant variable."
例如下面的：

> ```js
> var b = 10;
> (function b() {
>   'use strict'
>   b = 20;
>   console.log(b)
> })() // "Uncaught TypeError: Assignment to constant variable."
> ```

原因：
作用域：执行上下文中包含作用于链：
在理解作用域链之前，先介绍一下作用域，作用域可以理解为执行上下文中申明的变量和作用的范围；包括块级作用域/函数作用域；
特性：声明提前：一个声明在函数体内都是可见的，函数声明优先于变量声明；
在非匿名自执行函数中，函数变量为只读状态无法修改；

### 第 34 题：简单改造下面的代码，使之分别打印 10 和 20。

```js
var b = 10;
(function b(){
    b = 20;
    console.log(b); 
})();
```

答案

```js
var b = 10;
(function b() {
  const b = 20;
  console.log(b) //20
})()

console.log(b); // 10
(function a() {
    a = 20;
    console.log(b) //20
})()

(function b(b) {
    b.b = 20;
    console.log(b) //10
})(b)
```

### 第 35 题：请求时浏览器缓存 from memory cache 和 from disk cache 的依据是什么，哪些数据什么时候存放在 Memory Cache 和 Disk Cache中？ 

https://www.jianshu.com/p/54cc04190252

### 第 36 题：使用迭代的方式实现 flatten 函数

```js
function flatten(nums, depth = 9999) {
    if(!Array.isArray(nums)) {
        return [nums];
    } else if(depth === 1) {
        return nums;
    } else {
        const res = nums.reduce((pre, num) => {
            pre.push(...flatten(num, depth - 1));
            // 注意这里要 return
            return pre;
        },[])
        return res;
    }

}
console.log(flatten([1, [1, 2, [2, 3]]], 2))
```

ES10

```js
arr.flat(Infinity);
```

边界情况？

### 第 37 题：为什么 Vuex 的 mutation 和 Redux 的 reducer 中不能做异步操作？

vue用的不是很多，所以不是很清楚mutation里面为什么不能有异步操作，下面解释一下为什么Redux的reducer里不能有异步操作。

1. 先从Redux的设计层面来解释为什么Reducer必须是纯函数

如果你经常用React+Redux开发，那么就应该了解Redux的设计初衷。Redux的设计参考了Flux的模式，作者希望以此来实现时间旅行，保存应用的历史状态，实现应用状态的可预测。所以整个Redux都是函数式编程的范式，要求reducer是纯函数也是自然而然的事情，使用纯函数才能保证相同的输入得到相同的输入，保证状态的可预测。所以Redux有三大原则：

- 单一数据源，也就是state
- state 是只读，Redux并没有暴露出直接修改state的接口，必须通过action来触发修改
- 使用纯函数来修改state，reducer必须是纯函数

1. 下面在从代码层面来解释为什么reducer必须是纯函数

那么reducer到底干了件什么事，在Redux的源码中只用了一行来表示：

```
currentState = currentReducer(currentState, action)
```

这一行简单粗暴的在代码层面解释了为什么currentReducer必须是纯函数。currentReducer就是我们在createStore中传入的reducer（至于为什么会加个current有兴趣的可以自己去看源码），reducer是用来计算state的，所以它的返回值必须是state，也就是我们整个应用的状态，而不能是promise之类的。

要在reducer中加入异步的操作，如果你只是单纯想执行异步操作，不会等待异步的返回，那么在reducer中执行的意义是什么。如果想把异步操作的结果反应在state中，首先整个应用的状态将变的不可预测，违背Redux的设计原则，其次，此时的currentState将会是promise之类而不是我们想要的应用状态，根本是行不通的。

### 第 38 题：（京东）下面代码中 a 在什么情况下会打印 1？

```js
var a = ?;
if(a == 1 && a == 2 && a == 3){
 	console.log(1);
}
```

 答案：

```js
var a = {
    i : 1,
    toString(){
        return a.i++;
    }
};
if(a == 1 && a == 2 && a == 3){
 	console.log(1);
}
```

### 第 39 题：介绍下 BFC 及其应用。

BFC 就是块级格式上下文，是页面盒模型布局中的一种 CSS 渲染模式，相当于一个独立的容器，里面的元素和外部的元素相互不影响。创建 BFC 的方式有：

1. html 根元素
2. float 浮动
3. 绝对定位
4. overflow 不为 visiable
5. display 为表格布局或者弹性布局

BFC 主要的作用是：

1. 清除浮动
2. 防止同一 BFC 容器中的相邻元素间的外边距重叠问题

### 第 41 题：下面代码输出什么

```js
var a = 10;
(function () {
    console.log(a)
    a = 5
    console.log(window.a)
  	console.log(a)
    var a = 20;
    console.log(a)
})()
```

答案：undefined 10 20

代码等同于

```js
var a = 10;
(function () {
  	var a;
    console.log(a) // undefined
    a = 5 // 仅修改了局部变量
    console.log(window.a)// 全部变量还是5
    a = 20; // 局部变量赋值为 20 
    console.log(a)
})()
```

### 第 42 题：实现一个 sleep 函数

比如 sleep(1000) 意味着等待1000毫秒，可从 Promise、Generator、Async/Await 等角度实现

```js
// Promise
function sleep(time) {
    return new Promise(resolve => setTimeout(resolve, time)); 
}
sleep(3000).then(() => {
    console.log('Sleep Promise Done')
})

// async
function sleep(time) {
    return new Promise(resolve => setTimeout(resolve, time)); 
}
async function sleepAsync() {
    await sleep(3000)
    console.log('sleep Done')
}
sleepAsync()

//ES5
function sleep(callback,time) {
  if(typeof callback === 'function')
    setTimeout(callback,time)
}

function output(){
  console.log(1);
}
sleep(output,1000);
```

### 第 43 题：使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果

```js
const nums = [3, 15, 8, 29, 102, 22];
console.log(nums.sort((a, b) => {
    return a - b;
}))
```

### 第 44 题：介绍 HTTPS 握手过程

https://developers.weixin.qq.com/community/develop/article/doc/000046a5fdc7802a15f7508b556413

1. 客户端使用https的url访问web服务器,要求与服务器建立ssl连接
2. web服务器收到客户端请求后, 会将网站的证书(包含公钥)传送一份给客户端
3. 客户端收到网站证书后会检查证书的颁发机构以及过期时间, 如果没有问题就随机产生一个秘钥
4. 客户端利用公钥将会话秘钥加密, 并传送给服务端, 服务端利用自己的私钥解密出会话秘钥
5. 之后服务器与客户端使用秘钥加密传输

### 第 45 题：HTTPS 握手过程中，客户端如何验证证书的合法性

1、首先什么是HTTP协议?
http协议是超文本传输协议，位于tcp/ip四层模型中的应用层；通过请求/响应的方式在客户端和服务器之间进行通信；但是缺少安全性，http协议信息传输是通过明文的方式传输，不做任何加密，相当于在网络上裸奔；容易被中间人恶意篡改，这种行为叫做中间人攻击；
2、加密通信：
为了安全性，双方可以使用对称加密的方式key进行信息交流，但是这种方式对称加密秘钥也会被拦截，也不够安全，进而还是存在被中间人攻击风险；
于是人们又想出来另外一种方式，使用非对称加密的方式；使用公钥/私钥加解密；通信方A发起通信并携带自己的公钥，接收方B通过公钥来加密对称秘钥；然后发送给发起方A；A通过私钥解密；双发接下来通过对称秘钥来进行加密通信；但是这种方式还是会存在一种安全性；中间人虽然不知道发起方A的私钥，但是可以做到偷天换日，将拦截发起方的公钥key;并将自己生成的一对公/私钥的公钥发送给B；接收方B并不知道公钥已经被偷偷换过；按照之前的流程，B通过公钥加密自己生成的对称加密秘钥key2;发送给A；
这次通信再次被中间人拦截，尽管后面的通信，两者还是用key2通信，但是中间人已经掌握了Key2;可以进行轻松的加解密；还是存在被中间人攻击风险；

3、解决困境：权威的证书颁发机构CA来解决；
3.1制作证书：作为服务端的A，首先把自己的公钥key1发给证书颁发机构，向证书颁发机构进行申请证书；证书颁发机构有一套自己的公私钥，CA通过自己的私钥来加密key1,并且通过服务端网址等信息生成一个证书签名，证书签名同样使用机构的私钥进行加密；制作完成后，机构将证书发给A；
3.2校验证书真伪：当B向服务端A发起请求通信的时候，A不再直接返回自己的公钥，而是返回一个证书；
说明：各大浏览器和操作系统已经维护了所有的权威证书机构的名称和公钥。B只需要知道是哪个权威机构发的证书，使用对应的机构公钥，就可以解密出证书签名；接下来，B使用同样的规则，生成自己的证书签名，如果两个签名是一致的，说明证书是有效的；
签名验证成功后，B就可以再次利用机构的公钥，解密出A的公钥key1;接下来的操作，就是和之前一样的流程了；
3.3：中间人是否会拦截发送假证书到B呢？
因为证书的签名是由服务器端网址等信息生成的，并且通过第三方机构的私钥加密中间人无法篡改； 所以最关键的问题是证书签名的真伪；

4、https主要的思想是在http基础上增加了ssl安全层，即以上认证过程；:

### 第 46 题：输出以下代码执行的结果并解释为什么

```js
var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)
```

以下为个人猜想没有确切的理论依据：

> push() 方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

根据MDN的说法理解，`push`方法应该是根据数组的`length`来根据参数给数组创建一个下标为`length`的属性，我们可以做以下测试：
[![image](https://user-images.githubusercontent.com/6418374/55369081-9c8bbd80-5527-11e9-82c6-10eb6f09e32e.png)](https://user-images.githubusercontent.com/6418374/55369081-9c8bbd80-5527-11e9-82c6-10eb6f09e32e.png)

根据这个测试我们发现，`push`方法影响了数组的`length`属性和对应下标的值。
然后，正如楼上所说：

> 在对象中加入splice属性方法，和length属性后。这个对象变成一个类数组。

我们使用题目中的代码时得到了这个结果：
[![image](https://user-images.githubusercontent.com/6418374/55369152-ebd1ee00-5527-11e9-9898-2c03edbe5d36.png)](https://user-images.githubusercontent.com/6418374/55369152-ebd1ee00-5527-11e9-9898-2c03edbe5d36.png)

这个时候控制台输出的是一个数组，但是实际上它是一个伪数组，并没有数组的其他属性和方法，我们可以通过这种方法验证：
[![image](https://user-images.githubusercontent.com/6418374/55369209-2045aa00-5528-11e9-824c-8798132e7c81.png)](https://user-images.githubusercontent.com/6418374/55369209-2045aa00-5528-11e9-824c-8798132e7c81.png)

所以我认为题目的解释应该是：

1. 使用第一次push，obj对象的push方法设置 `obj[2]=1;obj.length+=1`
   2.使用第二次push，obj对象的push方法设置 `obj[3]=2;obj.length+=1`
   3.使用console.log输出的时候，因为obj具有 length 属性和 splice 方法，故将其作为数组进行打印
   4.打印时因为数组未设置下标为 0 1 处的值，故打印为empty，主动 obj[0] 获取为 undefined

第一第二步还可以具体解释为：因为每次push只传入了一个参数，所以 obj.length 的长度只增加了 1。push方法本身还可以增加更多参数，详见 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

**在对象中加入splice属性方法，和length属性后。这个对象变成一个类数组。**

### 47 VUE

### 48 题：call 和 apply 的区别是什么，哪个性能更好一些

1. Function.prototype.apply和Function.prototype.call 的作用是一样的，区别在于传入参数的不同；
2. 第一个参数都是，指定函数体内this的指向；
3. 第二个参数开始不同，apply是传入带下标的集合，数组或者类数组，apply把它传给函数作为参数，call从第二个开始传入的参数是不固定的，都会传给函数作为参数。
4. call比apply的性能要好，平常可以多用call, call传入参数的格式正是内部所需要的格式，参考[call和apply的性能对比](https://github.com/noneven/__/issues/6)

### 第 49 题：为什么通常在发送数据埋点请求的时候使用的是 1x1 像素的透明 gif 图片？



1. 没有跨域问题，一般这种上报数据，代码要写通用的；（排除ajax）

2. 不会阻塞页面加载，影响用户的体验，只要new Image对象就好了；（排除JS/CSS文件资源方式上报）

3. 在所有图片中，体积最小；（比较PNG/JPG）

   ----

   补充楼上~以下为参考了网上资料后的一些个人整理：

1. 能够完成整个 HTTP 请求+响应（尽管不需要响应内容）
2. 触发 GET 请求之后不需要获取和处理数据、服务器也不需要发送数据
3. 跨域友好
4. 执行过程无阻塞
5. 相比 XMLHttpRequest 对象发送 GET 请求，性能上更好
6. GIF的最低合法体积最小（最小的BMP文件需要74个字节，PNG需要67个字节，而合法的GIF，只需要43个字节）

### 第 50 题：（百度）实现 (5).add(3).minus(2) 功能。

```js
Number.prototype.add = function (num) {
    return this.valueOf() + num;
}
Number.prototype.minus = function(num){
    return this.valueOf() - num;
}
console.log((5).add(3).minus(2))
```





# NodeJS

# 计算机网络





# React

### style

style 应该是预先写好的 而不是字符串

Error: The `style` prop expects a mapping from style properties to values, not a string. For example, style={{marginRight: spacing + 'em'}} when using JSX.    in h1 (at src/index.js:22)    in div (at src/index.js:21)

```jsx
let classStr = ['abc','redBg'];//abc,redBg
```

正确的做法：使用join拼接:

```jsx
<h1  className={classStr.join(" ")} > Style </h1>//abc redBg
```

添加背景：

```jsx
let exampleStyle = {
  background:"skyblue",
  borderBottom:"1px solid red",
  //由于有-只能圈起来
  'background-image':"url(https://www.baidu.com/s?wd=2020%e5%b9%b4%e5%85%a8%e5%9b%bd%e4%b8%a4%e4%bc%9a&sa=ire_dl_gh_logo&rsv_dl=igh_logo_pc)"
}
//也可以这样并起来
  backgroundImage:"url(https://www.baidu.com/img/pc_cc75653cd975aea6d4ba1f59b3697455.png)",
```

## 基础

### 函数式组件

```jsx
function Childcom(){
  let title = <h2> subTitle </h2>
  return(
      <div>
        <h1> Hello world</h1>
        {title}
      </div>
  )
}


ReactDOM.render(
  <Childcom></Childcom>,
  document.getElementById('root')
);

```

### 状态：(React State)

相当于vue Data

下面老看一个时钟

```jsx
class Clock extends React.Component{
  //
  constructor(props){
    //这样之后才可以用this.props
    super(props);
    //状态(data)--->view

  }
  render(){
    //应该放在渲染函数里面进行渲染
    this.state = {
      time:new Date().toLocaleTimeString()
    }
    // console.log(this.state.time);
    return(
    <div>
      <h1> {this.state.time} </h1>
    </div>
    )
  }
}
```

注意：

```jsx
//如果反复进行一个组件的初始化，他是不会反复重建的
setInterval(()=>{ReactDOM.render(
  <Clock></Clock>,
  document.getElementById('root')
  )
  console.log(new Date())
}
,
1000);
```

### 使用生命周期函数对组件进行操作

```jsx
class Clock extends React.Component{
  //
  constructor(props){
    //这样之后才可以用this.props
    super(props);
    //状态(data)--->view
    this.state = {
      time:new Date().toLocaleTimeString()
    }
  }
  render(){
    //这会导致组件的耦合度很高

    // console.log(this.state.time);
    return(
    <div>
      <h1> {this.state.time} </h1>
    </div>
    )
  }
  //生命周期函数 渲染完成：
  componentDidMount(){
    setInterval(()=>{
      this.state.time = new Date().toLocaleString();
    },1000);
  }
}
```

但是上面的操作是不推荐的，

我们应该使用setState来进行局部的dom更新:

```jsx

  //生命周期函数 渲染完成：
  componentDidMount(){
    setInterval(()=>{
      console.log(this.state.time);
      //修改完之后并不会立即修改dom内容。
      //react会在这个函数内部所有设置的状态
      //等待内部所有完成之后进行更新
      //小程序也是借鉴react状态管理操作
      this.setState({
        time:new Date().toLocaleString()
      })
      console.log(this.state.time);
    },1000);
  }
```

### 父子数据传递

props 可以设置默认值：

HelloMessage.defaultProps = { name : 'yqLiu' msg:'helloworld' }



props可以传递函数，props传递父元素的函数，就可以去修改父元素的状态state,从而达到传递数据给父元素的效果。

#### props机制（父->子)

props的传值可以是任意的类型

注意props和State是两个不同的东西

```jsx
class ChildComponent extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div display={this.props.isActive?'block':'None'}> 
        ChildComponent
      </div>
    )
  }
}
```

#### 子传父

调用父元素的函数，从而实现数据的子传递父

```jsx
export default class ChildComponent extends Component {
  constructor(props) {
    super(props)
  
    this.state = {
       msg:'hi father'
    }
  }
  
  render() {
    return (
      <div>
        <button onClick={this.sendData}>传给父元素</button>
      </div>
    )
  }
  //使用箭头函数可以防止循环溢出
  sendData=()=>{
    this.props.getChildMsg(this.state.msg)
  }
}
```

注意不能直接使用传递过来的参数

```jsx
        {/* 注意 直接进行调用可能会导致循环渲染 */}
        <button onClick={this.props.getChildMsg('hello father')}>传给父元素</button>
```

使用箭头函数进行传参

```jsx
        {/* 要使用箭头函数进行传参 */}
        <button onClick={()=>this.props.getChildMsg('hello father')}>传给父元素</button>
```

### React事件

React 事件特点;

- 他绑定属性的命名是使用的驼峰命名法
- 如果采用jsx语法，用{}，传入一个函数，而不是字符串

事件对象：

```jsx
  parentEvent= (e)=>{
    console.log(e);
    //react的代理的原生事件对象
  }
```

普通的html中阻止默认行为 可以直接return false;

React中，必须prevetDefault

```jsx
  parentEvent= (e)=>{
    //react的代理的原生事件对象
    console.log(e.preventDefault);

    //由于是代理 所以不能直接returnfalse 不能拦截
    //使用preventDefault进行拦截
    e.preventDefault();
    return false;
  }
```

如何传参的时候不丢失事件？

```jsx
{/* 这样就变成了一个调用,使用箭头函数 使用e进行传参 */}
<button onClick={(e)=>this.parentEvent1('helloworld',e)}>submit</button>

  parentEvent1= (msg,e)=>{
    //react的代理的原生事件对象
    console.log(msg)
    console.log(e)
  }
```

注意不使用箭头函数会导致this的问题

```jsx
        {/* 不使用箭头函数要使用bind */}
        <button onClick={function(e){this.parentEvent1('helloworld',e)}.bind(this)}>submit</button>
```

### React条件渲染

- 直接通过条件判断返回需要渲染的jsx对象

判断是否登录：

```jsx
function UserGreet(props){
  return (<h1>Welcome</h1>);
}
function UserLogin(props){
  return (<h1>请先登录</h1>)
}

render() {
    if(this.state.isLogin){
      return (<UserGreet/>);
    }else{
      return (<UserLogin/>)
    }
```

注意 函数的引入必须使用component样式，不能直接return UserLogin- 

- 通过条件运算得出jsx对象，再将jsx对象渲染到模板中

```jsx
  render() {
    let ele = null;
    if(this.state.isLogin){
      ele =<UserGreet/>;
    }else{
      ele = <UserLogin/>
    }
    return (
      <div>
        <h1>yqLiu's Home</h1>
        {ele}
        <button onClick={()=>this.setState({isLogin:!this.state.isLogin})}>LogIn</button>
      </div>
    )
  
```

- 同样的，你也可以使用三元运算符：

```jsx
      <div>
        <h1>yqLiu's Home</h1>
        {this.state.isLogin?<UserGreet/>:<UserLogin/>}
        <button onClick={()=>this.setState({isLogin:!this.state.isLogin})}>LogIn</button>
      </div>
```

#### 列表渲染

将列表的内容拼装成数组 放置到模板当中

如果直接使用：

  ```jsx
let arr = ['代码补全','lsp解析','js学习文档'];    
{arr}
  ```

会输出

```jsx
代码补全lsp解析js学习文档
```

如果我们使用html对象

```jsx
let arr = [<li>'代码补全'</li>,'lsp解析','js学习文档'];
```



![image-20200521165437686](README.assets/image-20200521165437686-1590161113528.png)

可以使用map:

```jsx
    {arr.map((e)=>{return <YqLiuComponent childData={e}></YqLiuComponent>})}
```

效果：

![image-20200521170001124](README.assets/image-20200521170001124-1590161113528.png)

- 我们需要为每一个表单一个key属性：
  ![image-20200521195640661](README.assets/image-20200521195640661.png)

```jsx
        <ul>
          { this.state.list.map((e,index)=> {return (
            //加key
            <li key={e.title}>
              <h3> {index}:{e.title}</h3>
              <p>{e.content}</p>
            </li>
          )} ) }
        </ul>
```

得到一个jsx数组 然后放入到模板中。

Key值需要放置到每一项中。

#### 封装为组件

- 注意：使用简单的循环组件会报错：注意key的位置

```jsx
function ListItem(e){
  return (
    <li >
      <h3> {e.index}:{e.data.title}</h3>
      <p>{e.data.content}</p>
  </li>
  )
}  
Index.render() {
    return (
      <div>
        <h1>今日内容</h1>
        <ul>
          { this.state.list.map((e,index)=> {return (
            //key是要加在这里的
            <ListItem key={index} data={e} index={index}></ListItem>
          )} ) }
        </ul>
      </div>
    )
  }
```

### React制作疫情地图

除了数据处理的如下代码

```jsx
export default class List extends Component {
  constructor(props) {
    super(props)
  
    this.state = {
       
    }
  }
  
  render() {
    return (
      <div>
        <h1>China</h1>
        <ul>
          {provinceList.map((e)=>{
            return(
              <section>
                <h3>{`province:${e.province}`}</h3>
                <ul>
                  <li>
                    <span> 确诊 </span>
                    <span> 数量 </span>
                    <span> 疑似 </span>
                    <span> 死亡 </span>
                  </li>
                  <li>
                  {Object.keys(e).map(ele=>{
                    if(ele!='province')
                    return (
                      <span>{`${e[ele]}`}</span>
                    )
                  })}
                  </li>
                </ul>
              </section>

            )
          })}
        </ul>
      </div>
    )
  }
}
```

### React生命周期

声明周期的三个状态：

- Mounting(挂载) 将组件插入到dom对象中
- Updating：将数据更新到DOM中
- Ummounting:将组件移除dom

生命周期是从实例化到渲染到销毁的整个过程就是生命周期

在这个生命周期中有很多我们可以调用的事件

这就叫做

#### 钩子函数

- ComponentWillMount:将要渲染 Ajax 添加动画类
- ComponentDidMount:渲染完毕
- componentWillRecieveProps：组件将要接受props：查看props
- ShouldComponentUpdate:组件接收到新的state和props,判断是否更新，返回布尔值
- ComponentWillUpdate：组件将要更新
- ComponentDidUpdate:组件已经更新
- ComponentWillUnmount:组件将要卸载 保留一些状态或者数据

实例：

```jsx
class Child extends Component {
  constructor(props) {
    super(props)
  
    this.state = {
       msg:'helloworld'
    }
    console.log('constuctor Complete!')
  }
  UNSAFE_componentWillMount(){
    console.log('component will Mount')
  }
  componentDidMount(){
    console.log('component Did Mount')
  }
  UNSAFE_componentWillReceiveProps(){
    console.log("childData:"+this.props.childData);
    console.log('component will get props')
  }
  UNSAFE_componentWillUpdate(){
    console.log("childData:"+this.props.childData);
    console.log('component will update')
  }
  //通过判断来控制组件是否更新
  shouldComponentUpdate(){
    console.log('NotUpdate')
    //判断
    return true;
  }
  componentDidUpdate(){
    console.log("component did update")
  }
  componentWillUnmount(){
    console.log('component Died')
  }
  render() {
    console.log('render Start!')
    return (
      <div>
        {this.state.msg}
        <button onClick = {this.update}> Update </button>
      </div>
    )
  }
  update = ()=>this.setState({
    msg : 'hi the world'
  })
}
```

实现一个拦截更新：

```jsx
update = ()=>this.setState({
    msg : 'hi the world',
    update:false
  })  
shouldComponentUpdate(){
    console.log(`try to update ：Update Capacity${this.state.update}`)
    return this.state.update;
  }
```

### 使用echarts

我们首先要引入echart

```jsx
import echarts from 'echarts';
import 'echarts/map/js/china.js'
```



然后对echart在组件里面初始化，注意这里面应用了OnMount生命周期函数

```jsx
componentDidMount(){
    var myChart = echarts.init(document.getElementById('map'));
    let dataList = [];
    for (let ele of provinceList){
      dataList.push({
        name:ele.province,
        value:ele.value
      })
    }
    function randomValue() {
        return Math.round(Math.random()*1000);
    }
    let option = {
```

#### 写一个search组件

```jsx
export default class SearchCom extends Component {
    constructor(props) {
        super(props)
    
        this.state = {
             
        }
    }
    
    render() {
        return (
            <div>
                <input type="text" placeholder="请输入内容" onKeyDown={this.searchEvent} value=""/>
                <div>
                    <h2> 查询结果 </h2>
                </div>
            </div>
        )
    }
    searchEvent=(e)=>{
        console.log(e)
    }
}
```

![image-20200522151704390](README.assets/image-20200522151704390.png)

我们需要做一些事情来改变value值（change事件)

```jsx
<input type="text" placeholder="请输入内容" onKeyDown={this.searchEvent} value={this.state.value} onChange={this.changeEvent}/> 

searchEvent=(e)=>{
        if(e.keyCode===13){
            console.log(e.target.value);
            let _result = this.props.provinceObj[e.target.value]
            if(!_result){
                this.setState({
                result:<h2>4040NotFound</h2>
                });
            }else{
                this.setState({
                    result:
                    (<div>
                        <h1>{_result.province}</h1>
                        <h3>{_result.value}</h3>
                        <h3>{_result.dead}</h3>
                        <h3>{_result.suspect}</h3>
                    </div>)
                }) 
            }
        }
    }
```

### Ajax+React+Express+axios

服务端：

```js
const express = require('express');
const axios = require('axios');
const app = express();
app.get('/',(req,res)=>{
    res.send('I am a server')
})
app.get('/api/newsdata',async (req,res)=>{
    //请求头条数据
    let result = await axios.get('https://ng.ant.design/ngsw.json?ngsw-cache-bust=0.5633671768572814')
    let data = result.data;
    console.log(`data:${data}`)
    res.json(data)
})
app.listen(8080,()=>{
    console.log('server Start');
    console.log('localhost:8080');
    console.log('/api/newsdata');
}

)
```

## 进阶

### 插槽

语法：

```jsx
{this.props.children}

ReactDOM.render(
  <Parent>
    <h1>title</h1>
    <h1>title</h1>
    <h1>title</h1>
  </Parent>,
  document.getElementById('
    root')
)
```

控制插入的位置

```jsx
<ChildCom>
    <h1 data-position='header'>Header</h1>
    <h1 data-position='main'>Main</h1>
    <h1 data-position='footer'>Footer</h1>
</ChildCom>

export default class ChildCom extends Component {
  render() {
    let headerCom,mainCom,footCom;
    this.props.children.forEach((item,index)=>{
      switch(item.props['data-position']){
        case 'header':headerCom = item; break
        case 'main':mainCom = item; break
        case 'footer':footCom = item; break
      }
    })
    return (
      <div>
        {headerCom}
        {mainCom}
        {footCom}
      </div>
    )
  }
}
```

### 路由

单页面路由

```jsx
import{ BrowserRouter as Router,Link,Route,Redirect} from 'react-router-dom'
export default class Root extends Component {
  render() {
    return (
      <div>
        <Router basename='/admin'>
          <div className="nav">
            <Link to='/'>Home</Link>
            <Link to='/product'>product</Link>
            <Link to='/me'>me</Link>
          </div>
          <Route path='/' exact component={Home}></Route>
          <Route path='/product'  component={Product}></Route>
          <Route path='/me'  component={Me}></Route>
        </Router>
        <Router >
          <div className="nav">
            <Link to='/'>Home</Link>
            <Link to='/product'>product</Link>
            <Link to='/me'>me</Link>
          </div>
          <Route path='/' exact component={Home}></Route>
          <Route path='/product'  component={Product}></Route>
          <Route path='/me'  component={Me}></Route>
        </Router>
      </div>
    )
  }
}
```

Link组件 可以设置to属性to属性可以传递对象:

```jsx
            <Link to={{
              pathname:"/me",
              search:'?username=admin',
              hash:"#abc",
              state:{msg:'hellpworld'}
            }}>me</Link>
```

replace之后不能回退到上一个 例如 a-b-c c back ->a

```jsx
            <Link to='/product' replace>product</Link>
```

#### 动态路由

```jsx
<Link to='/news/456789' replace>News</Link>
<Route path='/news/:id'  component={News}></Route>
function News(props){
  console.log(props);
  return(
    <div>
      <h1>News</h1>
      <h3>News.id:{props.match.params.id}</h3>
    </div>
  )
}
```

#### 重定向(Redirect)

如果访问某个组件时，如果有充电像组件，那么就会修改页面的路

实例：

```jsx
function LoginInfo(props){
  //props.loginState = 'success|fail'
  console.log(props)
  if(props.location.state.loginState == 'success'){
    return <Redirect to='/admin'/>
  }
  return <Redirect to='/login'/>
}
let FormCom=()=>{
  let pathObj = {
    pathname:"/loginInfo",
    state:{
      loginState:'success'
    }
  }
  return (
    <div>
      <h1>Form!!!</h1>
      <Link to={pathObj}>login</Link>
    </div>
  )
}

export default class App extends Component {
  render() {
    return (
      <div>
        <Router>
          <Route path='/' exact component={()=>(<h1>首页</h1>)}></Route>
          <Route path='/login' exact component={()=>(<h1>登录</h1>)}></Route>
          {/* 表单验证 */}
          <Route path='/form' exact component={FormCom}></Route>
          <Route path='/LoginInfo' exact component={LoginInfo}></Route>
        </Router>
      </div>
    )
  }
}
```

#### Switch

加入一个abc对应了两个路由：

```jsx
<Route path='/login' exact component={()=>(<h1>登录</h1>)}></Route>
<Route path='/abc' exact component={()=>(<h1>abc1</h1>)}></Route>
```



![image-20200522204622585](README.assets/image-20200522204622585.png)

switch保证只匹配一个页面

```jsx
<Router>
          <Switch>
            <Route path='/' exact component={()=>(<h1>首页</h1>)}></Route>
            <Route path='/login' exact component={()=>(<h1>登录</h1>)}></Route>
            <Route path='/abc' exact component={()=>(<h1>abc1</h1>)}></Route>
            <Route path='/abc' exact component={()=>(<h1>abc2</h1>)}></Route>
            <Route path='/admin' exact component={()=>(<h1>admin,loginSuccess</h1>)}></Route>
            {/* 表单验证 */}
            <Route path='/form' exact component={FormCom}></Route>
            <Route path='/LoginInfo' exact component={LoginInfo}></Route>
          </Switch>
        </Router>
```

这样的话，只匹配一个

![image-20200522204823904](README.assets/image-20200522204823904.png)

#### 动态路由的匹配：

```jsx
<Route path='/:id' exact component={(props)=>{console.log(props);return <h1>首页</h1>}}></Route>
            <Route path='/abc' exact component={()=>(<h1>abc1</h1>)}></Route>
            <Route path='/abc' exact component={()=>(<h1>abc2</h1>)}></Route>

```



![image-20200522214049325](README.assets/image-20200522214049325.png)

#### 组件绑定跳转 

使用原生的push等操作进行组件跳转、

```jsx
clickEvent=()=>{
    console.log(this.props);
    //传入路径以及状态
    // this.props.history.push('/',{source:'fromChild'})
    // this.props.history.replace('/',{source:'fromChild'})
    this.props.history.go(1);
    this.props.history.goForward();
  }
```



### 状态管理Redux

Redux是一种解决方案，用于中大型，数据比较庞大，组件之间交互比较多的情况下使用。

#### Store

数据仓库

#### State

一个对象 包含了整个应用的所有数据

#### Action

动作：触发数据的改变方法。

Store.dispatch:将动作触发成方法。

Reducer:通过获取动作，改变数据，生成一个新的状态。生成一个新的state，从而改变页面

#### subscribe

订阅函数 用来进行多次刷新渲染

实例：

```jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom';
import Redux,{createStore} from 'redux'
//1、创建仓库
const store = createStore(reduce)
//reduce 生成状态的函数
const reduce = function(state={count:0},action){
  //4每次改变都会经过这个参数
  //4我们可以传入多个值，可以选取其中的一些修改数据
  console.log('action')
  switch(action.type){
    case 'add':state.count++;break;
    case 'decrease':state.count--;break;
    default: 
  }
  return state;
}

console.log(store)
// 3、封装函数到dispatch,调用reduce方法
function add(){
  //通过仓库的方法进行修改数据
  store.dispatch({type:'add'})
  console.log(store.getState().count)
}
function decrease(){
  store.dispatch({type:'decrease'})
  console.log(store.getState().count)
}
//函数式计数器
const Counter = function(props){
  return (
    <div>
      <h1>Count:{store.getState().count}</h1>
      {/* 2、应用 */}
      <button onClick={add}>count +1</button>
      <button onClick={decrease}>count -1</button>
    </div>
  )
}
ReactDOM.render(
  <Counter></Counter>,
  document.getElementById('root')
)
//5、多次渲染改变同步页面内容
store.subscribe(()=>{
  ReactDOM.render(
    <Counter></Counter>,
    document.getElementById('root')
  )
})
```

获取数据

```jsx
      <h1>Count:{store.getState().count}</h1>
```

修改数据

```jsx
  store.dispatch({type:'add'})
```

监听数据，修改视图

```jsx
store.subscribe(()=>{
  ReactDOM.render(
    <Counter></Counter>,
    document.getElementById('root')
  )
})
```

### react-redux


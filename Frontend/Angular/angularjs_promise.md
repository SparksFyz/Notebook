## Angularjs Promise & Design Patterns

### What is Promise

promise在angular中是一个接口,但它不是angular首创的,
而是作为一种编程模式,比js还要古老得多,promise全称是 Futures and promises。
有兴趣可以可以看看[Futures and promises](http://en.wikipedia.org/wiki/Futures_and_promises)。

在javascript中，promise是一种异步处理模式,用来解决异步编程问题.
Javascript中有一个库叫做[Q](https://github.com/kriskowal/q),而angularjs用promise实现的$q服务就是从Q引入的．


简单的状态图：

![promise](./image/promise.png)

Promise 背后的概念大致有两部分:

Deferreds，定义工作单元，
Promises，从 Deferreds 返回的数据。

Deferred对象可以看做是一个通信对象，用来定义工作单元的开始，处理和结束三部分。

Promise 是 Deferred 响应数据的输出；它有状态 (等待，执行和拒绝)，以及句柄，或叫做回调函数，反正就是那些在 Promise 执行，拒绝或者提示进程中会被调用的方法。

Promise 不同于回调的很重要的一个点是，你可以在 Promise 状态变成执行(resolved)之后追加处理句柄。这就允许你传输数据，而忽略它是否已经被应用获取，然后缓存它，等等之类的操作，因此你可以对数据执行操作，而不管它是否已经或者即将可用。




### 一个形象的栗子

> 关键词 : $q.defer()  $q.all()  $q.when()  deferred  deferred.resolve() ....

假设有一个家具厂，而它有一个VIP客户张先生。

有一天张先生需要一个豪华衣柜，于是，他打电话给家具厂说我需要一个衣柜，回头做好了给我送来，这个操作就叫$q.defer，也就是延期，因为这个衣柜不是现在要的，所以张先生这是在发起一个可延期的请求。

同时，家具厂给他留下了一个回执号，并对他说：我们做好了会给您送过去，放心吧。这叫做promise，也就是承诺。

这样，这个defer算是正式创建了，于是他把这件事记录在自己的日记上，并且同时记录了回执号，这叫做deferred，也就是已延期事件。

现在，张先生就不用再去想着这件事了，该做什么做什么，这就是“异步”的含义。

假设家具厂在一周后做完了这个衣柜，并如约送到了张先生家（包邮哦，亲），这就叫做deferred.resolve(衣柜)，也就是“已解决”。而这时候张先生只要签收一下这个（衣柜）参数就行了，当然，这个“邮包”中也不一定只有衣柜，还可以包含别的东西，比如厂家宣传资料、产品名录等。整个过程中轻松愉快，谁也没等谁，没有浪费任何时间。

假设家具厂在评估后发现这个规格的衣柜我们做不了，那么它就需要deferred.reject(理由)，也就是“拒绝”。拒绝没有时间限制，可以发生在给出承诺之后的任何时候，甚至可能发生在快做完的时候。而且拒绝时候的参数也不仅仅限于理由，还可以包含一个道歉信，违约金之类的，总之，你想给他什么就给他什么，如果你觉得不会惹恼客户，那么不给也没关系。

假设家具厂发现，自己正好有一个符合张先生要求的存货，它就可以用$q.when(现有衣柜)来把这个承诺给张先生，这件事就立即被解决了，皆大欢喜，张先生可不在乎你是从头做的还是现有的成品，只会惊叹于你们的效率之高。

假设这个家具厂对客户格外的细心，它还可能通过deferred.notify(进展情况)给张先生发送进展情况的“通知”。

这样，整个异步流程就圆满完成，无论成功或者失败，张先生都没有往里面投入任何额外的时间成本。

好，我们再扩展一下这个故事：

张先生这次需要做一个桌子，三把椅子，一张席梦思，但是他不希望今天收到个桌子，明天收到个椅子，后天又得签收一次席梦思，而是希望家具厂做好了之后一次性送过来，但是他下单的时候又是分别下单的，那么他就可以重新跟家具厂要一个包含上述三个承诺的新承诺，这就是$q.all(桌子承诺，椅子承诺，席梦思承诺)，这样，他就不用再关注以前的三个承诺了，直接等待这个新的承诺完成，到时候只要一次性签收了前面的这些承诺就行了。

> 上面这个栗子提到的promise就是angular中$q的实现

首先我们创建一个对象：
+ $q.defer()，用来创建一个新的 Deferred 对象
+ $q.all()，允许你等待并行的 promise 处理，当所有的 promise 都被处理结束之后，调用共同的回调
+ $q.when()，如果你想通过一个普通变量创建一个 promise ，或者你不清楚你要处理的对象是不是 promise 时非常有用

>$q.defer() 返回一个 Deferred 对象

Deferrred带有的方法和属性：

+ resolve(), reject(), 和 notify()

+ Deferred 还有一个 promise 属性，这是一个 promise对象，可以用于应用内部传递,这个对象有.then()这个方法，是唯一 Promise 规范要求的方法，
用三个回调方法作为参数；一个成功回调，一个失败回调，还有一个状态变化回调。对应上面的deferred.resolve(),deferrred.reject()和deferred.notify()

### 实际应用的栗子

>  创建一个service 用来向服务端请求数据

```javascript
App.factory('ProductInfo', ['$http', '$q', function ($http, $q) {
  return {
    query : function() {
      var deferred = $q.defer(); // 声明延后执行，表示要去监控后面的执行
      $http({method: 'POST', url: 'xxxx.php?r=xxController/xxAction'}).
      success(function(data, status, headers, config) {
        deferred.resolve(data);  // 声明执行成功，即http请求数据成功，可以返回数据了
      }).
      error(function(data, status, headers, config) {
        deferred.reject(data);   // 声明执行失败，即服务器返回错误
      });
      return deferred.promise;   // 返回承诺，这里并不是最终数据，而是访问最终数据的API
    } // end query
  };
}]);
```

> 在Controller中（以同步方式）使用这个Service

```javascript
angular.module('App')
  .controller('ProductCtrl', ['$scope', 'ProductInfo', function ($scope, ProductInfo) { // 引用我们定义的productInfo服务
    var promise = ProductInfo.query(); // 同步调用，获得承诺接口
    promise.then(function(data) {  // 调用承诺API获取数据 .resolve
        $scope.productInfo = data;
    }, function(data) {  // 处理错误 .reject
         //错误处理
    });
  }]);
```
> 这里有一个链式promise概念，我们可以在promise后追加多个处理(.then()):

例如：

```javacript
var deferred = $q.defer();
var promise = deferred.promise;

promise
  .then(function(val) {
    console.log(val);
    return 'B';
  })
  .then(function(val) {
    console.log(val);
    return 'C'
  })
  .then(function(val) {
    console.log(val);
   });

deferred.resolve('A');
```

output:

```
A
B
C
```

### Promise 相较于js中回调的优点
对比使用Promise前后我们可以发现，传统异步编程通过嵌套回调函数的方式，等待异步操作结束后再执行下一步操作。过多的嵌套导致意大利面条式的代码，可读性差、耦合度高、扩展性低。
通过Promise机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。


> 暂时还没有用过$q.all() 和$q.when() ,有机会再补充....


### 结合token ,Promise实现Interceptor

>　　以前写过一点这部分的代码，可惜搞丢了,过段时间补上来..


未完待续





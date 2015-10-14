## Controller as VS Scope In Angular Syntax

### Difference

> $Scope

```javascript
angular.module("app",[])

.controller("demoController",["$scope",function($scope){

    $scope.title = "angualr";

 }])

<div ng-app="app" ng-controller="demoController">
    hello : {{title}} !
</div>　　
```

> Controller as

```javascript
angular.module("app",[])

.controller("demoController",[function(){

    this.title = "angualr";

}])

<div ng-app="app" ng-controller="demoController as demo">
     hello : {{demo.title}} !
</div>　
```

> 通过 `controller as`， 不再需要注入$scope,controller更加靠近POJO，就是一个简单的javascript对象，在view model中可以通过controller的别名来访问数据对象。

关于`controller as` 的[源码](https://github.com/angular/angular.js/blob/c7a1d1ab0b663edffc1ac7b54deea847e372468d/src/ng/compile.js)

```javascript
if (directive.controllerAs) {

         locals.$scope[directive.controllerAs] = controllerInstance;

}　
```

> angular只是把controller这个对象实例以其as的别名在scope上创建了一个新的对象属性

###　栗子

```javascript
angular.module("app",[])

 .controller("demoController",[function(){
        var vm = this;<br>
        vm.title = "angualr";<br>
        return vm;
 }])
```

### What is scope

在 AngularJS 中，scope 是一个开放给 partial 的 JavaScript 对象。Scope 可以包含不同的属性 - 基本数据 (primitives)、对象和函数。所有归属于 scope 的函数都可以通过解析该 scope 所对应 partial 中的 AngularJS 表达式来执行，也可以由任何构件直接调用该函数 (使用这种方式将保留指向该 scope 的引用不变)。附属于 scope 的数据可以通过使用合适的 directive 来绑定到视图上，如此一来，所有 partial 中的修改都会映射为某个 scope 属性的变化，反之亦然。

AngularJS 应用中的 scope 还有另一个重要的特质，即它们都被连接到一条原型链 (prototypical chain) 上 (除了那些被表明为独立 (isolated) 的 scope)。在这种方式中，任何子 scope 都能调用属于其父母的函数，因为这些函数是该 scope 的直接或间接原型的属性。

### Why use `controller as`?

1. 定义vm这样会让我们更好的避免JavaScript的this的坑。
2. 如果某个版本的angular不再支持controller as,可以轻易的注入$scope,修改为 var vm = $scope;
3. 因为不再注入$scope了，controller更加的POJO，就是一个很普通的JavaScript对象。
4. 也因为没有了$scope，而controller实例将会成为scope上的一个属性，所以在controller中我们再也不能使用$watch,$emit,$on之类的特殊方法，因为这些东西往往不该出现在controller中的，更好的控制。
5. 因为controller实例将会只是$scope的一个属性，所以view模板上的所有字段都会在一个引用的属性上，这可以避开JavaScript原型链继承对于值类型的坑。[related to](https://github.com/angular/angular.js/wiki/Understanding-Scopes).
6. controller as 对于 coffescript,liveScript更友好。
7. 模板上定义的每个字段方法都会在scope寄存在controller as别名上的引用上，所以在controller继承中，不会在出现命名冲突的问题。


## Summary About Angularjs

### Basic

> angular的数据绑定采用什么机制？

- angular 通过监控能事件改变进行绑定；DOM事件(input改变，点击等)，XHR的响应触发回调；浏览器地址变化，计时器触发回调等；如果以上情况没有发生，MV改变的可能性不大;

> 两个平级界面块a和b，如果a中触发一个事件，有哪些方式让b知道？

- 两个controller之间进行交互可以通过父作用域数据共享，或者事件的冒泡广播机制；

> 一个angular应用应当如何良好地分层？

- 将controller中的东西可以向JAVA代码一样进行层次划分，数据请求的dao层，数据处理的service层。很明显提问者应该是后端开发者，大部分只做前端开发的应该对此理解不深；

> angular应用常用哪些路由库，各自的区别是什么？

- ui.router; angular原生的不支持嵌套路由，一个路由只能对应页面中一个 区域。ui.router 还没有遇到不支持的功能。

> 分属不同团队进行开发的angular应用，如果要做整合，可能会遇到哪些问题，如何解决？

-  服务冲突，angular将所有模块的所有服务混入了应用级别的单一命名空间里。开发时规划好，尽量避免冲突；

> angular的缺点有哪些？

> 如果通过angular的directive规划一套全组件化体系，可能遇到哪些挑战？

> 如何看待angular1.2中引入的controller as语法？ 能解决什么样的问题？(scope)

- 参见 [Link](https://github.com/SparksFyz/Notebook/blob/master/Frontend/Angular/controller_as.md)

> angular依赖注入

-  angular会将依赖的所有模块上的所有服务混入应用级别的单一命名空间内。所以，模块间可以相互依赖，不同模块间的服务可以相互依赖。这有一个问题就是会存在冲突的可能性。在需要使用的地方根据名称注入。 

> 如何看待angular2


### Specific

> ng-if与ng-show/hide的区别？
 
- ng-if会通过判断来决定是否在页面上生成DOM元素.    

> ng-repeat迭代数组的时候，如果数组中有相同值，会有什么问题，如何解决？

- 默认情况下ng-repeat是不允许存在相同值的，这是因为不能管理数据与DOM之间一对一的关系了；但是如果必须存在相同值，可以通过指定 track by $index用于区别不同数据

> ng-click中写的表达式，能使用js原生对象上的方法，比如Math.max之类的吗？

- ng-click中写的表达式不能使用比如Math.max等JS原生对象上的方法,这是因为执行时如$scope.Math.max

> {{now|'yyyy-MM-dd'}}这种表达式里，竖线和后面的参数通过什么方式可以自定义？

- [filterProvider](https://code.angularjs.org/1.4.5/docs/api/ng/provider/$filterProvider)

> factory和service,provide的关系

- factory和service，provider都用于注册服务，不同的是provider只能配置阶段注入，factory和service只能运行阶段注入；provider通过$get工厂函数穿件新对象,factory通过工厂函数创建新对象，service通过构造函数创建新对象.

>  directive 如何调用外部函数，如何向函数里传递参数，如何expose函数给外部调用?

> 
html: {{currentDate()}} js: $scope.currentDate = function(){return new Date();} 这种写法有没有问题(digest)?





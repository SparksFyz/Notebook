## Angularjs framework optimizing

### 按需加载静态资源

#### angularjs + angular-couch-patato + requirejs

- index.html

```html
<script src="require.js" data-main="main.js?v=1.0"></script>
```

- main.js

```js
require.config({
    baseUrl: 'xxx',
    paths: {
             'jquery' : 'assets/js/jquery.min',
           'angular' : 'assets/js/angular'},
    shim: {
      angular: {
        deps: ['jquery'],
        exports: 'angular'
      }
    }
});
```

- route.js

```js
$stateProvider
    .state('index', {
        url: 'index/',
        templateUrl: 'xxxx.tpl.html',
        resolve: {
            ctrl: $couchPotatoProvider.resolveDependencies(['home.ctrl'])
        }
    })
```
>    Lazyload机制


Lazyload机制可以使状态流转的时候去按需加载页面所需的资源，而不是在一开始就加载。AngularJS本身没有实现lazyload，这样的话就导致了webapp的性能受影响，在首次加载的时候就要加载全部的依赖。

>    define angular module

```script
var app = angular.module('app', ['scs.couch-potato', 'ui.router', 'ui.bootstrap', 'ui.bootstrap.tpls', 'chieffancypants.loadingBar']);
```
上述代码片段展示了Angular module的加载机制。而在实际应用中，特别是业务系统，一般是业务比较繁杂，功能模块比较多，在这种情况下，angular module的默认机制就不符合我们要求了。

因此，我们采用requirejs + angular-couch-patato来实现Lazyload。angular-couch-patato负责托管angular的各种注册，包括controller,directive,filter,service等。平时我们写

```script
angular.module('app').controller(function () {
    //ctrl code here
    });
```

> To use LazyLoad,we need to change as follows:

```script
angular.module('app').registerController(function () {
    //ctrl code here
    });
```

>将部件注册，这样所有的部件都交给couch-patato来管理，在需要的时候加上使用requirejs，app是统一管理的angular module，我们就这样写：

```script
define(['app'], function(app) {
    app.registerController(function () {
    //ctrl code here
    });
 })
```

> 首先加载依赖app，app是之前定义好的angular module，如var app = angular.module('app', ['scs.couch-potato', 'ui.router', 'ui.bootstrap', 'ui.bootstrap.tpls', 'chieffancypants.loadingBar']);。然后在app上注册controller等。

>上文路由机制部分提到，在注册路由的时候需要调用$couchPotatoProvider.resolveDependencies来实现lazyload。这里设计到couchPatato实现的一个关键，就是利用angular在路由里面的resolve机制，这里的resolve是指，路由在触发之前，必须拿到resolve里面定义的所有值。这样couchPatato就有机会去获取定义的依赖，而在每个依赖里面，会将controller注册到app，然后在路由触发后，定义的controller才得以调用。


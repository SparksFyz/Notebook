## The issue about multiple directives on the same element

### 一个元素上多指令的相关问题


### 1. 一个元素上多个指令的scope创建

```javascript
app.registerDirective 'ngSelectRow',[
    '$rootScope'
    ($rootScope) ->
      return (
        restrict: 'A'
        //scope:
        link: (scope, elem, attrs) ->
        )
  ]
```
> scope has three optional value

- scope: false   : 默认值，指令不会新建一个作用域，使用父级作用域。
- scope: true    : 指令会创建一个新的子作用域，原型继承于父级作用域。
- scope: { ... } : 指令会新建一个独立作用域，不会原型继承父作用域。

每个指令对于 Scope 的行为都是不一样的,当某一个元素上有多个指令共存时,以上的三种情况简称为false指令,true指令和isolated指令。

> 当三种指令共存时的比较:


+ false and true

  false指令使用父级Scope，true指令创建新Scope，两者共存时将共享新建的Scope，并且该Scope原型继承自父级Scope。

+ false and isolated

  false指令使用父级Scope，isolated指令会新建的独立Scope。两者共存时同样会为isolated指令创建独立Scope，并将此独立Scope用于isolated指令的link函数；false指令的link使用的是父级Scope，但是对于该元素下子元素有效的仍然是父级Scope。也就是说新建的独立Scope会被忽略，子级元素的编译（compile）使用的仍然是父级Scope。

+ true and isolated

  这两种指令都会创建新的Scope，但是类型不同，两者共存时就会发生编译错误：

```console
Error: [$compile:multidir] Multiple directives [isolatedScope, trueScope] asking for new/isolated scope
```

+ multiple true

  多个true指令共存时，只会创建一个新的Scope，该Scope继承自父级Scope，并且被这些指令共享。

> If multiple directives on the same element request a new scope, only one new scope is created.

+ multiple false

  这种情况对于 Scope 不会有什么影响，都是使用的父级 Scope ，并没有什么特殊之处。

+ multiple isolated

  多个isolated指令共存时，都要求创建新的独立 Scope ，也会发生编译报错：

```console
Error: [$compile:multidir] Multiple directives [isolated1Scope, isolated2Scope] asking for new/isolated scope
```

+ true and false and isolated

  isolated指令与true指令共存时会报错，所以这种情况也很容易想清楚，他们共存时也会报错。
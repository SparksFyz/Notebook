### Event Binding

1. bind
2. live
3. delegate
4. on

> `on` and `off` is the only method recommended after version 1.7 in JQuery
and other methods are all implemented by `on` and `off`.


#### bind 缺点:

- 它不会绑定到在它执行完后动态添加的那些元素上
- 当元素很多时，会出现效率问题
- 当页面加载完的时候，你才可以进行bind()，所以可能产生效率问题

#### 栗子

```javascript
// Bind
$( "#members li a" ).on( "click", function( e ) {} );
$( "#members li a" ).bind( "click", function( e ) {} );

// Live
$( document ).on( "click", "#members li a", function( e ) {} );
$( "#members li a" ).live( "click", function( e ) {} );

// Delegate
$( "#members" ).on( "click", "li a", function( e ) {} );
$( "#members" ).delegate( "li a", "click", function( e ) {} );
```


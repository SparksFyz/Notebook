### Binding style of list item selected in table and ul.

#### Scene

> 有一个数据列表，点中其中某条，这条就改变样式变成加亮，如果用传统的方式，可能要添加一些事件，
  然后在其中作一些处理，但使用数据绑定，能够大幅简化代码.

#### 栗子

```js
function ListCtrl($scope) {
    $scope.items = [];

    for (var i=0; i<10; i++) {
        $scope.items.push({
            title:i
        });
    }

    $scope.selectedItem = $scope.items[0];

    $scope.select = function(item) {
        $scope.selectedItem = item;
    };
}
```

```html
<ul class="list-group" ng-controller="ListCtrl">
    <li ng-repeat="item in items" ng-class="{true:'list-group-item active', false: 'list-group-item'}[item==selectedItem]" ng-click="select(item)">
        {{item.title}}
    </li>
</ul>
```




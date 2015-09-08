## Traits

### As of PHP 5.4.0, PHP implements a method of code reuse called Traits.

> Traits 是一种为类似 PHP 的单继承语言而准备的代码复用机制。Trait 为了减少单继承语言的限制，使开发人员能够自由地在不同层次结构内独立的类中复用方法集。Traits 和类组合的语义是定义了一种方式来减少复杂性，避免传统多继承和混入类（Mixin）相关的典型问题。

>  Trait 和一个类相似，但仅仅旨在用细粒度和一致的方式来组合功能。Trait 不能通过它自身来实例化。它为传统继承增加了水平特性的组合；也就是说，应用类的成员不需要继承。

---------------------------------------------------------------------------------------------

### Precedence

#### Example a

```php
<?php
class Base {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait SayWorld {
    public function sayHello() {
        parent::sayHello();
        echo 'World!';
    }
}

class MyHelloWorld extends Base {
    use SayWorld;
}

$o = new MyHelloWorld();
$o->sayHello();
?>
```

> output : Hello World!

> An inherited method from a base class is overridden by the method inserted into MyHelloWorld from the SayWorld Trait. The behavior is the same for methods defined in the MyHelloWorld class. The precedence order is that methods from the current class override Trait methods, which in turn override methods from the base class.

#### Example b

```php
<?php
trait HelloWorld {
    public function sayHello() {
        echo 'Hello World!';
    }
}

class TheWorldIsNotEnough {
    use HelloWorld;
    public function sayHello() {
        echo 'Hello Universe!';
    }
}

$o = new TheWorldIsNotEnough();
$o->sayHello();
?>
```

> output : Hello Universe!

--------------------------------------------------------------------------------------------------------

### Multiple Traits

#### Example c (Multiple Traits Usage)

```php
<?php
trait Hello {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait World {
    public function sayWorld() {
        echo 'World';
    }
}

class MyHelloWorld {
    use Hello, World;
    public function sayExclamationMark() {
        echo '!';
    }
}

$o = new MyHelloWorld();
$o->sayHello();
$o->sayWorld();
$o->sayExclamationMark();
?>
```

> output : Hello World!

-----------------------------------------------------------------------------------

### 一个栗子

> \_\_CLASS\_\_  will return the name of the class in which the trait is being used (!) not the class in which trait method is being called:

```php
<?php
trait TestTrait {
    public function testMethod() {
        echo "Class: " . __CLASS__ . PHP_EOL;
        echo "Trait: " . __TRAIT__ . PHP_EOL;
    }
}

class BaseClass {
    use TestTrait;
}

class TestClass extends BaseClass {

}

$t = new TestClass();
$t->testMethod();

//Class: BaseClass
//Trait: TestTrait
```



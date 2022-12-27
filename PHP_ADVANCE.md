[When should I use 'self' over '$this'](https://stackoverflow.com/questions/151969/when-should-i-use-self-over-this)

 $this refer current object

 self refer current class
 
 $this->member refer non-static members
 
 self::$member refer-static members

```php
<?php
class X {
    private $non_static_member = 1;
    private static $static_member = 2;

    function __construct() {
        echo $this->non_static_member . ' ' . self::$static_member;
    }
}

new X();
?>
```

to call parent class static methods/properties
```php
class Bar extends Foo
{
    public function fooStatic() {
        return parent::$my_static;
    }
}
```

[PHP Why return new static? ](https://stackoverflow.com/questions/37460592/why-return-new-static-php)

new static instantiates a new object from the current class (instantiates the subclass if the class was subclassed).

Having a static method on a class which returns a new instance of same is an alternative constructor.
```php
class Foo {
    public function __construct(BarInterface $bar, array $baz = []) { ... }

    public static function fromBarString($bar) {
        return new static(new Bar($bar));
    }

    public static function getDefault() {
        return new static(new Bar('baz'), [42]);
    }

}
```
[New self vs. new static](https://stackoverflow.com/questions/5197300/new-self-vs-new-static)

```php
class A {
    public static function get_self() {
        return new self();
    }

    public static function get_static() {
        return new static();
    }
}

class B extends A {}

echo get_class(B::get_self());  // A
echo get_class(B::get_static()); // B
echo get_class(A::get_self()); // A
echo get_class(A::get_static()); // A
```

```php
class alpha {
    function classname(){
        return __CLASS__;
    }

    function selfname(){
        return self::classname();
    }

    function staticname(){
        return static::classname();
    }
}

class beta extends alpha {

    function classname(){
        return __CLASS__;
    }
}

$beta = new beta();
echo $beta->selfname(); // Output: alpha
echo $beta->staticname(); // Output: beta
//However, if we remove the declaration of the classname function from the beta class, we get 'alphaalpha' as the result.
```

[PHP anonymous function vs Regular functions](https://www.elated.com/php-anonymous-functions/)

**anonymous function**

*There is no function name*
*There is a semicolon after the function definition*

**How Useful**

*Assign it to a variable, then call it later using the variable’s name*
*Pass it to another function that can then call it late*
*Return it from within an outer function so that it can access the outer function’s variables.

[closure vs callable](https://stackoverflow.com/questions/29730720/type-hinting-difference-between-closure-and-callable/40942212#40942212)

*The difference is, that a Closure must be an anonymous function, where callable also can be a normal function.*

```php
function callFunc1(Closure $closure) {
    $closure();
}

function callFunc2(Callable $callback) {
    $callback();
}

function xy() {
    echo 'Hello, World!';
}

callFunc1("xy"); // Catchable fatal error: Argument 1 passed to callFunc1() must be an instance of Closure, string given
callFunc2("xy"); // Hello, World!
```
[PHP Why return new static? ](https://stackoverflow.com/questions/37460592/why-return-new-static-php)

new static instantiates a new object from the current class (instantiates the subclass if the class was subclassed).

Having a static method on a class which returns a new instance of same is an alternative constructor.
```php
class Foo {
    public function __construct(BarInterface $bar, array $baz = []) { ... }
}
```
Having an alternative constructor allows you to provide different defaults, or convenience shortcuts to instantiate this class without having to supply 
those specific arguments and/or for being able to provide different arguments which the alternative constructor will convert to the canonical ones:
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

[New self vs. new static](https://stackoverflow.com/questions/16977369/php-new-staticvariable)

static in this case means the current object scope. It is used in late static binding.

Normally this is going to be the same as using self. The place it differs is when you have a object heirarchy where the reference to the scope is defined on a parent but is being called on the child. self in that case would reference the parents scope whereas static would reference the child's

```php

class A{
    function selfFactory(){
        return new self();
    }

    function staticFactory(){
        return new static();
    }
}

class B extends A{
}


$b = new B();

$a1 = $b->selfFactory(); // a1 is an instance of A

$a2 = $b->staticFactory(); // a2 is an instance of B
```
It's easiest to think about self as being the defining scope and static being the current object scope.

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
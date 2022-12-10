**rescue()**

*The rescue function executes the given closure and catches any exceptions that occur during its 
execution. All exceptions that are caught will be sent to your exception handler; however, 
the request will continue processing. Second argument will be the "default" value that should be
 returned if an exception occurs while executing the closure*

```php
return rescue(function () {
    return $this->method();
}, false);
 
return rescue(function () {
    return $this->method();
}, function () {
    return $this->failure();
});
```

**wrap()**

*the make() method create a new instance of a collection from an array. wrap() method is similar
 to make() method, except it will create a collection from any value supplied to it*
 
 ```php
$product = new Product();
$products = Collection::wrap($product);

function addProductsToOrder($products) {
    // Ensure that the products is always a collection.
    Collection::wrap($products)->each->addToOrder();
}
```

**unwrap() is opposite**

*, if the provided value is already a collection, the collection’s underlying items will be returned.
 If not then the value will be returned as it is without modification.*
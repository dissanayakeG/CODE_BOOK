**with()**

*we can name the method anything you want with with() in laravel*
```php
    return view('index')
        ->withName('Name')
        ->withFullName('Full Name')
        ->withaddress('Your address')
        ->withcountryName('CountryName');
```

*can access the values in view explained below*
```
    withName('Name') in view it becomes $name
    withFullName('Full Name') in view it becomes $fullName
    withaddress('Your address') in view it becomes $address
    withcountryName('CountryName') in view it becomes $countryName
```

*All of the syntax below archives the same thing
```
    return view('blog')->withBlogs($blogs);    
    return view('blog')->with('blogs', $blogs);    
    return view('blog')->with(compact('blogs'));    
    return view('blog', compact('blogs'));
```


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
 
 - resolve()
 - config()
 - get_data()
 - set_date()
 - Str::of()
 - Str::markdown()
 - Str::start()
- 
 - tap()
 - pipe()
 - throw_if()
 - throe_unless()
 - match()
 - collect()
 
 
```php
 $value = Cache::remember('users', $seconds, function () {
     return DB::table('users')->get();
 });
```
 
*If the item does not exist in the cache, the closure passed to the remember method will be executed and its result 
will be placed in the cache.*
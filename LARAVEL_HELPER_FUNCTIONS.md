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

 - throw_if()
 - throe_unless()
 - match()

 **whenNotEmpty()**
 -The whenNotEmpty method will execute the given callback when the collection is not empty:
 
 **whenEmpty()**
 -The whenEmpty method will execute the given callback when the collection is empty:

 **when()**
 -The when method will execute the given callback when the first argument given to the method evaluates to true. The collection instance and the first argument given to the when method will be provided to the closure:

 -A second callback may be passed to the when method. The second callback will be executed when the first argument given to the when method evaluates to false:


**search()**
-The search method searches the collection for the given value and returns its key if found. If the item is not found, false is returned:


**pipe()**
-The pipe method passes the collection to the given closure and returns the result of the executed closure:

```php
    $collection = collect([1, 2, 3]);
    
    $piped = $collection->pipe(function ($collection) {
        return $collection->sum();
    });
    
    // 6
```

**shift()**
-The shift method removes and returns the first item from the collection:

```php
    $collection = collect([1, 2, 3, 4, 5]); 
    $collection->shift();    
    // 1    
    $collection->all();    
    // [2, 3, 4, 5]

    $collection2 = collect([1, 2, 3, 4, 5]); 
    $collection2->shift(3);    
    // collect([1, 2, 3])    
    $collection2->all();    
    // [4, 5]
```



**tap()**
-The tap method passes the collection to the given callback, allowing you to "tap" into the collection at a specific point and do something with the items while not affecting the collection itself. The collection is then returned by the tap method:

```php
    collect([2, 4, 3, 1, 5])
    ->sort()
    ->tap(function ($collection) {
        Log::debug('Values after sorting', $collection->values()->all());
    })
    ->shift();
 
// 1
```
















```php
 $value = Cache::remember('users', $seconds, function () {
     return DB::table('users')->get();
 });
```
 
*If the item does not exist in the cache, the closure passed to the remember method will be executed and its result 
will be placed in the cache.*
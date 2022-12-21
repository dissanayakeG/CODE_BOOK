**computed properties**

```php
//coomputed property name surrounded by get and Property keywords
public function getFirstNameProperty(){
    return 'some';
}

//in blade
{{$firstName}}

//from inherited classes or when property in a trait
use traitName;
dd($this->first_name);
```


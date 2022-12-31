**Laravel's Container**

*Behind the scenes Laravel resolves all your controller classes from the Container, that's the reason you can 
type hint any class and get an instance really easily.*

```php
App::make('Slack\Api'); // Using the App Facade
app('Slack\Api'); // Using the app helper function
resolve('Slack\Client'); // Using the resolve helper function

<?php

namespace Slack;
class Api
{
    public function __construct(Class $dependency)
    {
        $this->dependency = $dependency;
    }
}
```

*Not all dependencies will be resolvable by the Container*

*you can pass an array as the second parameter with any data that should be sent to the class*

```php
<?php

namespace Calebporzio\Onboard;

class OnboardingManager
{
    public function __construct($user, OnboardingSteps $onboardingSteps)
    {
        $this->steps = $onboardingSteps->steps($user);
    }
}
```
*we aren't type hinting the $user dependency, so the Container wouldn't know how to fetch this dependency for us*

*This is the trait to be added to the app's User class.*
```php
<?php
namespace Calebporzio\Onboard;
use Illuminate\Support\Facades\App;

trait GetsOnboarded
{
    public function onboarding()
    {
        return App::make(OnboardingManager::class, ['user' => $this]);
    }
}
```
*we manually pass in the value for the $user dependency*

**Facades**

*facade is a class that provides access to an object from the container*
```php
use Illuminate\Support\Facades\Cache;
 
class UserController extends Controller
{
    public function showProfile($id)
    {
        $user = Cache::get('user:'.$id);
 
        return view('profile', ['user' => $user]);
    }
}

class Cache extends Facade
{
    //This method's job is to return the name of a service container binding
    protected static function getFacadeAccessor() { return 'cache'; }
}
//service provider

public function register()
{
    $this->singletonAndAlias('cache', Cache::class);
}
```

**Real-Time Facades**

*To generate a real-time facade, prefix the namespace of the imported class with Facades*
```php
<?php 
use Facades\App\Contracts\Publisher;//this is realtime facade
use Illuminate\Database\Eloquent\Model;
 
class Podcast extends Model
{
    public function publish()
    {
        $this->update(['publishing' => now()]); 
        Publisher::publish($this);
    }
}
```

**Service containers**

[Service container vs Service provider](https://stackoverflow.com/questions/54018582/laravel-service-container-and-service-provider#:~:text=Service%20container%20is%20where%20your,adding%20them%20to%20the%20container.)

*service container is a powerful tool for managing class dependencies and performing dependency injection*

//container bindings will be registered within service providers
```php
use App\Services\Transistor;
use App\Services\PodcastParser;
 
$this->app->bind(Transistor::class, function ($app) {
    return new Transistor($app->make(PodcastParser::class));
});
```

*if you would like to interact with the container outside of a service provider, you may do so via the App facade*

```php
use App\Services\Transistor;
use Illuminate\Support\Facades\App;
 
App::bind(Transistor::class, function ($app) {
    // ...
});
````

**Add additional methods to collection**
```php
use Illuminate\Support\Collection;
use Illuminate\Support\Str;
 
Collection::macro('toUpper', function () {
    return $this->map(function ($value) {
        return Str::upper($value);
    });
});
 
$collection = collect(['first', 'second']);
 
$upper = $collection->toUpper();
 
// ['FIRST', 'SECOND']
````
*should declare collection macros in the boot method of a service provider*

[Laravel macros](https://levelup.gitconnected.com/understanding-laravel-macros-e2f493484a38)

*Laravel macros allow us to add custom functionality to Laravel core components or classes. In other words, they allow us to extend Laravel classes*

**Laravel macros**

allow  add custom functionality to Laravel core components or classes.
ex: 
```php
Response::macro('csv', function ($headers, $rows, $filename) {});

//now his can be used as
return response()->csv($headers, $rows, $filename);
```

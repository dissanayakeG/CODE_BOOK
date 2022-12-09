**rendering a view in browser**

```php
Route::get('/', function () {
    return view('welcome', ['name' => 'Samantha']);
});
```

**using variables**

```php
Hello, {{ $name }}
```

**include php scripts**

```php
@php
    $counter = 1;
@endphp
```

**example blade directives**

```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don t have any records!
@endif
```
```php
@unless (Auth::check())
    You are not signed in.
@endunless
```
```php
@isset($records)
    // $records is defined and is not null...
@endisset

@empty($records)
    // $records is "empty"...
@endempty
```
```php
@auth
    // The user is authenticated...
@endauth

@guest
    // The user is not authenticated...
@endguest
```
```php
@auth('admin')
    // The user is authenticated...
@endauth

@guest('admin')
    // The user is not authenticated...
@endguest
```
```php
@production
    // Production specific content...
@endproduction
```
```php
@env('staging')
    // The application is running in "staging"...
@endenv

@env(['staging', 'production'])
    // The application is running in "staging" or "production"...
@endenv
```
```php
@switch($i)
    @case(1)
        First case...
        @break

    @case(2)
        Second case...
        @break

    @default
        Default case...
@endswitch
```

**LOOPS**

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I am looping forever.</p>
@endwhile

loop variables avalable

@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is the first iteration of the parent loop.
        @endif
    @endforeach
@endforeach

$loop->index/iteration/remaining/count/first/last/even/odd/depth/parent
```

**Conditional Classes**

```php
@php
    $isActive = false;
    $hasError = true;
@endphp

<span @class([
    'p-4',
    'font-bold' => $isActive,
    'text-gray-500' => ! $isActive,
    'bg-red' => $hasError,
])></span>

<span class="p-4 text-gray-500 bg-red"></span>
```

**Including Subviews**

```php
@include('shared.errors')

use cases of including views
@include('view.name', ['status' => 'complete'])
@includeIf('view.name', ['status' => 'complete'])
@includeWhen($boolean, 'view.name', ['status' => 'complete'])

@includeUnless($boolean, 'view.name', ['status' => 'complete'])
@includeFirst(['custom.admin', 'admin'], ['status' => 'complete']) //To include the first view that exists from a given array of views
```

**Rendering Views For Collections**

```php
@each('view.name', $jobs, 'job', 'view.empty')
args-->view_name, data_array_current_value,view that will be rendered if the given array is empty
```

# Components

`php artisan make:component Alert`

_components are automatically discovered within the app/View/Components directory and resources/views/components directory_

_Manually Registering Package Components when building a package_

```php
service provider
public function boot()
{
    Blade::component('package-alert', Alert::class);
}

usecase
<x-package-alert/>

public function boot()
{
    Blade::componentNamespace('Nightshade\\Views\\Components', 'nightshade');
}
<x-nightshade::calendar />
```

**Passing Data To Components**

```php
<x-alert type="error" :message="$message"/>
```

_All public properties and methods on a component will automatically be made available to the component's view._

```php
public function isSelected($option){}
<span {{ $isSelected($value)></span>
```

_Component constructor arguments should be specified using camelCase, while kebab-case should be used when referencing the argument names in your HTML attributes._

```php
public function __construct($alertType)
{
    $this->alertType = $alertType;
}

<x-alert alert-type="danger" />
```

_use :: to inform Blade that the attribute is not a PHP expression_

_usecase_

```php
<x-button ::class="{ danger: isDeleting }">
    Submit
</x-button>
```

**render method clouser acceppt $data array**

```php
public function render()
{
    return function (array $data) {
        // $data['componentName'];
        // $data['attributes'];
        // $data['slot'];

        return '<div>Components content</div>';
    };
}
```

**Hiding Attributes / Methods**

```php
class Alert extends Component
{
    public $type;
    protected $except = ['type'];
}
```

_By {{ $attributes }} can get all the atributes of component_

**Default / Merged Attributes**

```php
<div {{ $attributes->merge(['class' => 'alert alert-'.$type]) }}>
    {{ $message }}
</div>

<x-alert type="error" :message="$message" class="mb-4"/>
output--> <div class="alert alert-error mb-4"></div>
```

**Conditionally Merge Classes**

```php
<div {{ $attributes->class(['p-4', 'bg-red' => $hasError]) }}>
    {{ $message }}
</div>

chaning
<button {{ $attributes->class(['p-4'])->merge(['type' => 'button']) }}>
    {{ $slot }}
</button>
```

**Non-Class Attribute Merging**

_the values provided to the merge method will be considered the "default" values of the attribute_
_these attributes will not be merged with injected attribute values_
_Instead, they will be overwritten_

```php
<button {{ $attributes->merge(['type' => 'button']) }}>
    {{ $slot }}
</button>

<x-button type="submit">
    Submit
</x-button>

<button type="submit">
    Submit
</button>
```

**Retrieving & Filtering Attributes**

```php
{{ $attributes->filter(fn ($value, $key) => $key == 'foo') }}
{{ $attributes->whereStartsWith('wire:model') }}
{{ $attributes->whereDoesntStartWith('wire:model') }}
{{ $attributes->whereStartsWith('wire:model')->first() }}
@if ($attributes->has('class'))
    <div>Class attribute is present</div>
@endif
{{ $attributes->get('class') }}
```

**anonymous components**

_anonymous components do not have any associated class_

```php
@props(['type' => 'info', 'message'])

<div {{ $attributes->merge(['class' => 'alert alert-'.$type]) }}>
    {{ $message }}
</div>

<x-alert type="error" :message="$message" class="mb-4"/>
```

**Accessing Parent Data by @aware**

```php
<x-menu color="purple">
    <x-menu.item>...</x-menu.item>
    <x-menu.item>...</x-menu.item>
</x-menu>

<!-- /resources/views/components/menu/index.blade.php -->

@props(['color' => 'gray'])

<ul {{ $attributes->merge(['class' => 'bg-'.$color.'-200']) }}>
    {{ $slot }}
</ul>

<!-- /resources/views/components/menu/item.blade.php -->

@aware(['color' => 'gray'])

<li {{ $attributes->merge(['class' => 'text-'.$color.'-800']) }}>
    {{ $slot }}
</li>
```

**Dynamic Components**

```php
<x-dynamic-component :component="$componentName" class="mt-4" />
```

# SLOTS AND LAYOUT EXAMPLE
```php
<!-- resources/views/components/layout.blade.php -->
<html>
    <head>
        <title>{{ $title ?? 'Todo Manager' }}</title>
    </head>
    <body>
        <h1>Todos</h1>
        <hr/>
        {{ $slot }}
    </body>
</html>

<!-- resources/views/tasks.blade.php -->
<x-layout>
    <x-slot name="title">
        Custom Title
    </x-slot>
 
    @foreach ($tasks as $task)
        {{ $task }}
    @endforeach
</x-layout>

*these content will be rendered inside {{ $slot }} of layout.blade.php*

use App\Models\Task;

Route::get('/tasks', function () {
return view('tasks', ['tasks' => Task::all()]);
});
```

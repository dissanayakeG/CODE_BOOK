- In Alpine.js, you cannot directly manipulate the data in the x-init attribute. Instead, you should use x-init to call a function that will perform the initialization.

- Dont use `'init()'` as `x-init` function name, it will call init function multiple times.
If the `x-data` object of a component contains an `init()` method, it will be called automatically. For example:

- Internally, Livewire's frontend is built on top of Alpine. In fact, every Livewire component is actually an Alpine component under-the-hood. This means that you can freely utilize Alpine inside your Livewire components.

- Livewire exposes a magic $wire object to Alpine.

- The `$wire` object can be treated like a JavaScript version of your Livewire component.

- When an Eloquent model is assigned to a Livewire component property, Livewire will automatically lock the property and ensure the ID isn't changed
`php public Post $post; `

- Computed properties are methods in your component marked with the `#[Computed]` attribute. They can be accessed as a dynamic property that isn't stored as part of the component's state but is instead evaluated on-the-fly.

- The reason is that computed properties have a performance advantage, since they are automatically cached after their first usage during a single request. This means you can freely access `$this->todos` within your component and be assured that the actual method will only be called once, so that you don't run an expensive query multiple times in the same request.

- Because wire: uses Alpine's `x-on` directive under the hood, these modifiers are made available to you by Alpine.

###Don't trust action parameters
- Action parameters should be treated just like HTTP request input, meaning action parameter values should not be trusted. You should always authorize ownership of an entity before updating it in the database.

- By default, Livewire will automatically disable submit buttons and mark inputs as readonly while a form is being submitted, preventing the user from submitting the form again while the first submission is being handled.

- When using Livewire, every component is an island. This means that when an update is triggered on the parent and a network request is dispatched, only the parent component's state is sent to the server to re-render - not the child component's. The intention behind this behavior is to only send the minimal amount of data back and forth between the server and client, making updates as performant as possible.

- If you want or need a prop to be reactive, you can easily enable this behavior using the 

[Reactive] attribute parameter.
```php
    <livewire:todo-count :$todos />
    	
	class TodoCount extends Component
	{
	    #[Reactive] 
    	    public $todos;
	}
```

- Livewire provides a `#[Modelable]` attribute you can add to any child component property to make it modelable from a parent component.(for sharing state between parent and child components)

`#[Modelable]` attribute added above the $value property to signal to Livewire that if wire:model is declared on the component by a parent it should bind to this property:
	
```php
    	<livewire:todo-input wire:model="todo" /> 

		class TodoInput extends Component
		{
		    #[Modelable] 
		    public $value = '';
		 
		    public function render()
		    {
			return view('livewire.todo-input');
		    }
		}
		
		<div>
		    <input type="text" wire:model="value" >
		</div>
```

- The alternative to a single page application is a multi-page application. In these applications, every time a user clicks a link, an entirely new HTML page is requested and rendered in the browser.

```php
<a href="/livewire">{{ "LIVEWIRE" }}</a> This relod the page.
<a href="/livewire" wire:navigate>{{ "LIVEWIRE" }}</a> This not.
return $this->redirect('/posts', navigate: true);
```

###Reloading when assets change
- But, now that you are using `wire:navigate` and each page visit is no longer a fresh browser page load, your users may still be receiving stale JavaScript after deployments.

- To prevent this, you may add data-navigate-track to a `<script> tag in <head>:`

```php
	<!-- Page one -->
	<head>
	    <script src="/app.js?id=123" data-navigate-track></script>
	</head>
	 
	<!-- Page two -->
	<head>
	    <script src="/app.js?id=456" data-navigate-track></script>
	</head>
```

- When a user visits page two, Livewire will detect a fresh JavaScript asset and trigger a full browser page reload.

		
- If you are using Laravel's Vite plug-in to bundle and serve your assets, Livewire adds data-navigate-track to the rendered HTML asset tags automatically. You can continue referencing your assets and scripts like normal:

```php
	<head>
	    @vite(['resources/css/app.css', 'resources/js/app.js'])
	</head>
```

- Livewire will only reload a page if a `[data-navigate-track]` element's query string (?id="456") changes, not the URI itself `(/app.js)`.
Scripts in `<head>` are loaded once	
New `<head>` scripts are evaluated (if includes a new JavaScript library)
Scripts in the `<body>` are re-evaluated

-However, unlike a normal component load, a lazy component has to serialize or "dehydrate" any passed-in properties and temporarily store them on the client-side until the component is fully loaded.

```php
	<livewire:revenue lazy :$start :$end />

	public function mount($start, $end)
	{
		// Expensive database query...
		$this->amount = Transactions::between($start, $end)->sum('amount');
	}
```






		
		
		

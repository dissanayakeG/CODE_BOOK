art make:component AppLayout

move component/app-layout.blade into new layout directory

rename app-layout.blade to app.blade

 public function render()
    {
        return view('layout.app');
    }
    
copy everything in welcome.blade into app.blade.php

app.blade 

<body>
	<div>
   	 {{$slot}}
	</div>
</body>

welcome.blade

<x-app-layout>
    <div>
        some some some
    </div>
</x-app-layout>

render new component with navigations
=====================================
artisan make:livewire Collection

web.php
Route::get('/collection', \App\Http\Livewire\Collection::class);

Collection.php
public function render()
    {
        return view('livewire.collection') ->layout('layout.app');
    }
    
    collection.blade
    
    <div>
    Hello Foo! Welcome to the register page!
</div>



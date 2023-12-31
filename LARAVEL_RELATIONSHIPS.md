> tutorial : https://www.youtube.com/playlist?list=PL1JpS8jP1wgA7YIkG5pJDa0XwvonK-mQR

> When using hasMany/through, the function name should be in the plural form; otherwise, use the singular form.

###hasOne/hasMany

```php
    $this->hasOne/many->(Modal::class, 'Other tbl link ID to me')
```

###belongsTo

```php
    $this->belongsTo->(Modal::class, 'Parent ref:ID of my table')
    //if using naming convention, functionName_id
```

###Has One Through / Has Many Through

1. Plumber has many Clients
2. Clients belongs to Plumber
3. Client has many Referrals
4. Referral belongs to Client
5. Plumber has many Referrals Through Clients

```php

Plumber modal
-------------
1. $this->hasMany(Client::class)
5. $this->hasManyThrough(Referral::class, Client::class, 'plumber_id', 'client_id')
//2nd param is intermediary class
//plumber_id in Client and client_id in Referral
//order is client through referral

Client Modal
------------
2. $this->belongsTo(Plumber::class)

3. $this->hasMany(Referrals::class)

Referral Modal
--------------
4. $this->belongsTo(Client::class)
```

###BelongsToMany/ManyToMany (no soft deletes) //use sync,attach,detach methods

Parent : region | store
Pivot : region_store //singular form of alphabetical order
//php artisan make:migration create_region_store_table --create=region_store

```php
>param 1 : related modal name
>param 2 : pivot table name
>param 3 : pivot table fk to me
>param 4 : pivot table fk to related table

//From storeModal

public function regions()
{
    $this->belongsToMany->(Region::class, 'pivotTableName', 'stores_id', 'regions_id')
            ->as('customName') //options | so instead of access data like ->pivot->name, can access like >customName->name
            ->using(PivotModal::class) //optional
            ->withPivot('active') //optional
            ->withTimestaps() //optional
}
```

> group_user, pivot tbl name should in singular alphabetical order, if we have followed naming convention, 2nd parameter is optional
> 3rd and 4th params are foreign keys

###Polymorphic (One to One)

-   Joe's car rentals

-   Employee can rent a Car
-   Customer can rent a Car

-   parents : Employee & Customer
-   Child : Car

```php
Mapping Models in Service Provider
----------------------------------
    public function boot(){
        Relation::morphMap([
            'Article' => 'App\Models\Article'
        ]);
    }

Car migration
-------------
    $table->morphs('carable');
    // or
    $table->unsignedBigInteger('carable_id');
    $table->string('carable_type'); //Customer or Employee //This can be aliased in a service provider; otherwise, it saves the full path of the model in the database column.

Car model
---------
    public function carable() : MorphTo
    {
        return $this->morphTo();
    }

Customer/Employee Model
----------------------
    public function car() : MorphOne
    {
        return $this->morphOne(Car::class, 'carable');
    }

    $employee = Employee::first()->car()->create([....]);
```

###Polymorphic (One to Many)

-   Community website

-   Articles can have many comments
-   Threads can have many comments

-   Parents: Articles & Threads
-   Child: Comments

```php

Comments migration
------------------
    $table->morphs('commentable');

Comments Model
--------------
    public function commentable() : MorphTo
    {
        return $this->morpTo();
    }

Article/Thread Model
----------------------
    public function comments() : MorphMany
    {
        return $this->morphMany(Comment::class, 'commentable');
    }

```

###Polymorphic (Many to Many)

-   Articles/threads can have many tags, and a tag can belong to many articles/threads.

```php

//need both tags and taggable tables

taggable migration
------------------
    $table->foreignId('tag_id')->constrained('tags');
    $table->morphs('taggable');

Tag Model
---------
    public function threads() : MorphToMany
    {
        return $this->morphedByMany(Thread::class, 'taggable');
    }

    public function articles() : MorphToMany
    {
        return $this->morphedByMany(Article::class, 'taggable');
    }

Article/Thread Model
----------------------
    public function tags() : MorphToMany
    {
        return $this->morphToMany(Tag::class, 'taggable');
    }

$article = Article::first()->tags()->attach(1)
$tag = Tag::first()->articles()->first()
```

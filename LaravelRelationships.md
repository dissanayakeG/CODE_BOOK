https://www.youtube.com/playlist?list=PL1JpS8jP1wgA7YIkG5pJDa0XwvonK-mQR

>When using hasMany/through, the function name should be in the plural form; otherwise, use the singular form.


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

```php
    $this->belongsToMany->(Modal::class, 'pivotTableName', 'group_id', 'user_id')
            ->as('customName') //so instead of access data like ->pivot->name, can access like >customName->name
            ->using(PivotModal::class)
            ->withPivot('active')
            ->withTimestaps()
```
            
>group_user, pivot tbl name should in singular alphabetical order, if we have followed naming convention, 2nd parameter is optional
>3rd and 4th params are foreign keys
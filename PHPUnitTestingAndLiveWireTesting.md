#LiveWire
**Simulating user loging to system**
```php
$this->actingAs(User::factory()->create());
```

**check that the page exists and get a 200 response**

```php
$this->get('/profile/edit')
 ->assertStatus(200);
```

**check livewire component rendered on web**
```php
$this->get('/profile/edit')
->assertSeeLivewire('edit-profile');
```

**test db model/create functions**
```php
        $user = User::factory()->create();

        $profile = Profile::factory()
            ->forUser($user)
            ->create();
        
        $this->assertInstanceOf(Profile::class, $profile);
//forUser method is on profile factory
```

**test db model/belongsTo relationship functions**
```php
        $user = User::factory()->create();

        $profile = Profile::factory()
            ->forUser($user)
            ->create();
        
        $this->assertInstanceOf(Profile::class, $profile);
        $this->assertEquals(
            $user->getKey(),
            $profile->user->getKey()
        );

//user ralationship is on profile.php
```

**test db model/user can have profile**
```php
        $profile = Profile::factory()
            ->forUser($user = User::factory()->create())
            ->create();

        $this->assertInstanceOf(Profile::class, $profile);

        $this->assertEquals(
            $profile->getKey(),
            $user->profile->getKey()
        );
//profile ralationship is on User.php
```

**asserting that we can set an ‘bio’ field on the Profile model**
```php
        $profile = Profile::factory()
            ->forUser($user = User::factory()->create())
            ->withBio($bio = 'Lorem ipsum')
            ->create();

        $this->assertInstanceOf(Profile::class, $profile);

        $this->assertEquals(
            $bio,
            $profile->bio,
        );
//ProfileFactory

    public function withBio(string $bio)
    {
        return $this->state(function () use ($bio) {
            return [
                'bio' => $bio,
            ];
    });

```
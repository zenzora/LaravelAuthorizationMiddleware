# LaravelAuthorizationMiddleware
Middleware to link permissions to route middleware on laravel 5

A small class to help authorize routes using laravel permissions, useful in situations where all that is required is an authenticated user.
If the user does not have the permission specified (or is unauthenticated) it will return a 404

<strong>Installation:</strong>

Add the Authorization Class to App/Http/Middleware

Edit App/Http/Kernal.php and add the following line to the middleware array at the bottom
```php
'permission' => \App\Http\Middleware\Authorization::class,
```
Generate a new autoload file (you can run 'composer dumpautoload' from your projects directory if you have composer installed)

<strong>Usage:</strong>

Create your permissions like so (https://laravel.com/docs/5.1/authorization#defining-abilities)

This middleware is useful for permissions that only take in an authenticated user model for example
```php
$gate->define('do_an_admin_thing', function ($user) {
            return $user->isAdmin();
        });
```

Edit your Routes.php file to include this middleware and pass on the permissions name like so

```php
Route::group(['middleware' => ['auth']], function() {
    Route::get('/', 'HomeController@index')->name('home.index');
    Route::group(['middleware' => ['permission:do_an_admin_thing']], function() {
        Route::get('admin/adminThing1', 'AdminController@adminThing1');
        Route::get('admin/adminThing2', 'AdminController@adminThing2');
    });
});
// Authentication Routes...
$this->get('login', 'Auth\AuthController@showLoginForm')->name('auth.login');
$this->post('login', 'Auth\AuthController@login');
$this->get('logout', 'Auth\AuthController@logout')->name('auth.logout');
```

        

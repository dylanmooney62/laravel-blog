---
title: Posts - Routing
date: "2021-12-06"
description: Setting up routes, authentication middleware and scaffolding the PostController
---

## Routing

All Post CRUD functionality should only be accessible to authenticated users with the following roles: **admin**, **editor**, or **author**. However, I will only implement authentication at this stage of the development process. I will implement authorization after the Post CRUD functionality has been completed to avoid unneeded complexity at such an early stage.


All routes relating to administrative functionality on the site will be protected by authentication can be found at the **/admin** route.

```php
// web.php
Route::group(['prefix' => 'admin', 'as' => 'admin.', 'middleware' => 'auth'], function () {
    Route::resource('posts', PostController::class);
});
```

The routing is done using the `group` method. 

The prefix is to `'admin'` which prefixes all the nested with **/admin**. 

`'as'` sets the name of the route `'admin.'` this allows me to reference the route when performing redirects and generating URLs to resources i.e.

```php 
route('admin.posts')
```

Finally, the middleware is set to the auth. This middleware will run on every request that starts with **/admin**. This middleware will ensure the user is logged in before accessing the resource. Otherwise, it will redirect the user to the login page.

The routes for **posts** can all be done in one line thanks to the `resource` method. This method creates standard URL paths for CRUD operations and maps them to the specified class.

```php
Route::resource('posts', PostController::class);
```

The following **posts** resource will be mapped like this:

| Verb        | URI                   | Action      | Route Name           |
| ----------- | -------------         | ----------- | -------------------- |
| GET         | /photos               | index       | admin.photos         |
| GET         | /photos/create        | create      | admin.photos.create  |
| POST        | /photos               | store       | admin.photos.store   |
| GET         | /photos/{photo}       | show        | admin.photos.show    |
| GET         | /photos/{photo}/edit  | edit        | admin.photos.edit    |
| PUT/PATCH   | /photos/{photo}       | update      | admin.photos.update  |
| DELETE      | /photos/{photo}       | destroy     | admin.photos.destroy |


## Scaffolding

Before the aforementioned routes will work we will need to create the `PostController`

Laravel makes this super fast and simple using the `make:controller` Artisan command:

```
php artisan make:controller AdminControllers/PostController --resource
```



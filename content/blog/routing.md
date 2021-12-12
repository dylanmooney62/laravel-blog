---
title: Post Routing
date: "2021-12-02 01"
description: Setting up routes, authentication middleware and scaffolding the PostController.
---

## Routing

All Post CRUD functionality should only be accessible to authenticated users with the following roles: **admin**, **editor**, or **author**. 

However, I will only implement authentication at this stage of the development process. I will implement authorization after the Post CRUD functionality has been completed to avoid unneeded complexity at such an early stage.


All routes relating to the administrative functionality of the site will be protected by authentication and can be found at the /admin route.

```php
// web.php
Route::group(['prefix' => 'admin', 'as' => 'admin.', 'middleware' => 'auth'], function () {
    Route::resource('posts', PostController::class);
});
```

The routing is done using the group method. 

The prefix is admin which prefixes all the nested resources with /admin. 

'as' sets the name of the route admin this allows me to reference the route when performing redirects and generating URLs to resources i.e.

```php 
route('admin.posts')
```

Finally, the middleware is auth. This middleware will run on every request that starts with /admin. This middleware will ensure the user is logged in before accessing the resource. Otherwise, it will redirect the user to the login page. 

The routes for posts can all be done in one line thanks to the resource method.

This method creates standard URL paths for CRUD operations and maps them to the specified class.

```php
Route::resource('posts', PostController::class);
```

The following posts resource will be mapped like this:

| Verb        | URI                   | Action      | Route Name           |
| ----------- | --------------------- | ----------- | -------------------- |
| GET         | /posts                | index       | admin.posts          |
| GET         | /posts/create         | create      | admin.posts.create   |
| POST        | /posts                | store       | admin.posts.store    |
| GET         | /posts/{post}         | show        | admin.posts.show     |
| GET         | /posts/{post}/edit    | edit        | admin.posts.edit     |
| PUT/PATCH   | /posts/{post}         | update      | admin.posts.update   |
| DELETE      | /posts/{post}         | destroy     | admin.posts.destroy  |


## Scaffolding

Before the aforementioned routes will work I will need to create the PostController

Laravel makes this super fast and simple using the make:controller Artisan command:

```
php artisan make:controller AdminControllers/PostController --resource
```

The way guests and administrators view Posts will be different, so a PostController has been created in the AdminController directory.

This allows for a separation of concerns.


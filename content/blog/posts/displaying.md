---
title: Posts - Displaying
date: "2021-12-06"
description: Setting up routes, authentication middleware and scaffolding the PostController
---


## Displaying Posts

Displaying posts is simple. On the index method we return a view, retrieve the latest and assign them to the posts variable

```php
// AdminControllers/PostController.php
class PostController extends Controller
{

    public function index()
    {
        return view('admin.posts.index', [
            'posts' => Post::latest()->get()
        ]);
    }

    //...

```

Using the blade templates we can then iterate over each post and generate HTML markup

```php
@foreach ($posts as $post)
<tr class="">
    <td>
        <a href={{route('admin.posts.edit', $post)}} class="font-bold hover:underline">
            {{$post->title}}
        </a>
    </td>
    <td>
        <div class="badge badge-lg">
            {{$post->category->name}}
        </div>
    </td>
    <td>{{$post->author->name}}</td>
    <td>{{$post->created_at->toFormattedDateString()}}</td>
    <td class="text-right">
        <a href={{route('admin.posts.edit', $post)}} class="link mr-2">Edit</a>
        <button class="link text-accent" @click="open('{{$post->title}}', '{{$post->id}}')">
            Delete
        </button>
    </td>
</tr>
@endforeach
```


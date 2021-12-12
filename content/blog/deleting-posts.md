---
title: Deleting Posts
date: "2021-12-04 01"
description: Improving the user experience when deleting a post with Alpine.js.
---

As deleting a post is destructive and permanent, a good UI/UX will implement error prevention allow the user to back track on their decision.

For this I want to implement a modal that will appear every time the user clicks the delete button. 

For this I used the minimal Alpine.js framework laravel provides out of the box

First, I created the markup for the modal:

```html
<div id="modal" class="modal" :class="show && 'modal-open'" :show="show">
    <div class="modal-box -mt-60">
        <div class="text-center">
            <p>Are you sure you wish to delete post titled: <strong x-text="title"></strong> <br> This action
                cannot be
                undone.
            </p>
        </div>
        <div class="modal-action">
            <button class="btn btn-ghost" @click="close">Back</button>
            <form :action="action" method="POST">
                @method('DELETE')
                @csrf
                <button class="btn btn-error">Delete</button>
            </form>
        </div>
    </div>
</div>
```

As you can there are multiple attributes that will be bound to dynamic data:

- class
- show
- title
- action

Next I create an alpine object that contains all the data for modal logic.


```html
<!-- resources/views/admin/posts/index.blade.php -->
<script>
document.addEventListener('alpine:init', () => {
    Alpine.data('modal', () => ({
        show: false,
        title: '',
        action: '',

        open(title, id) {
            this.action = `posts/${id}`
            this.title = title;
            this.show = true;
        },

        close() {
            this.show = false;
        }
    }))
})
</script>
```

To understand how this works we take a look at the button element for each post

```html
<button class="link text-accent" @click="open('{{$post->title}}', '{{$post->id}}')">
     Delete
</button>
```

As you can see clicking the button will call the **open** method and pass the title and id of the post.

The open method will then use this id to fill in the action and title for the modal.

Show will be set to true and display the modal on the screen.

For example, clicking on the delete button for this post will generate the following markup: 

![Single post in table](../../src/images/ui/deleting-a-post.png)

```php
<div id="modal" class="modal modal-open" :class="show 'modal-open'" :show="show" show="true">
            <div class="modal-box -mt-60">
                <div class="text-center">
                    <p>Are you sure you wish to delete the post titled: <strong x-text="title">My First Post</strong> <br> This action
                        cannot be
                        undone.
                    </p>
                </div>
                <div class="modal-action">
                    <button class="btn btn-ghost" @click="close">Back</button>
                    <form :action="action" method="POST" action="posts/3">
                        <input type="hidden" name="_method" value="DELETE">
                        <input type="hidden" name="_token" value="utuu4fYaxXCXW747UjjQasdSfrrj5IDuYs3BT61h">                        
                    <button class="btn btn-error">Delete</button>
            </form>
        </div>
    </div>
</div>
```

As you can seen the form data has has been filled with the id of the post. So when clicking the delete button the post with the id of **3** will be deleted.

This is displayed like this to the user:

![Delete modal for the post titled: My first post](../../src/images/ui/delete-modal-v2.png)


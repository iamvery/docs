---
name: Realtime Comments
desc: Creating the chat-based comment system.
---

Now that we have statistics that auto-update, let's add the comment feature. We
want this to feel more like a live chat, so we'll make use Pakyow UI again.

## Data Layer

First, we need a comment mutable to fetch and alter our comments state. Create a
file named `app/lib/mutables/comments_mutable.rb` and add the following code:

```ruby
Pakyow::App.mutable :comment do
  query :for_default_post do
    redis.lrange(:comments, 0, -1).each_with_index.map { |content, id|
      {
        id: id + 1,
        content: content
      }
    }
  end

  action :create do |params|
    redis.lpush(:comments, params[:content])
  end
end
```

The `for_default_post` query finds all comments and creates an array of hashes
from the raw data. Under it, the `create` action creates a new comment in Redis.

## View Logic

Next, we need to define the mutator for view rendering. Create a
`app/lib/mutators/comments_mutator.rb` file and add the following code:

```ruby
Pakyow::App.mutators :comment do
  mutator :list do |view, data|
    view.apply(data)
  end
end
```

This is almost exactly like our `stats` mutator, except we're calling the
`apply` view transformation method rather than `bind`. The `apply` method
transforms the view to completely match the data and then binds the values in.

When we apply an empty collection of comments, `apply` will simply render the
default version of the scope. If the collection contains one or more comments,
it will create a copy of the comment scope for each comment. The result is that
the view always contains the same number of elements as the collection being
applied, while providing some convenience around handling empty collections.

## Routing

Now that we have our mutable and mutator, it's time to wire it up. Open
`app/lib/routes.rb` and add the following code at the end of the default route:

```ruby
# render the comment list
view.partial(:'comment-list').scope(:comment).mutate(:list,
  with: data(:comment).for_default_post
).subscribe

# setup the form for a new object
view.partial(:'comment-form').scope(:comment).bind({})
```

We first render our comment list with the list of comments and subscribe it to
future state changes. Then we bind an empty object to the comment form, so that
Pakyow will setup the form for creating a new comment.

Next, we need to define the restful route for comment creation. Add the
following after the default route definition:

```ruby
restful :comment, '/comments' do
  action :create do
    # create the comment
    data(:comment).create(params[:comment])

    # go back
    redirect :default
  end
end
```

This route will be called when a `POST` request is sent to `/comments`. Now
there's only one more step. We need to tell Pakyow to tie the comment scope to
the restful resource. Open `app/lib/bindings.rb` and replace the contents with
the following code:

```ruby
Pakyow::App.bindings do
  scope :comment do
    restful :comment
  end
end
```

Now everything is hooked up.

## Avoiding the Form Submission Refresh

If you open [localhost:3000](http://localhost:3000) in two browsers you'll see
that comments created in one window automatically show up in the other window.
This is really nice, but it's still a bit clunky for the comment creator since
the form submission causes a full page refresh.

Fortunately the Pakyow client library, Ring, gives us a way to avoid this. Open
`app/views/_comment-form.html` and add the following `data-ui` attribute to the
form node:

```html
<form data-scope="comment" data-ui="mutable">
```

This attaches a ui component named `mutable` to the form node. The mutable
component ships with Ring, and interprets a user's interaction with the
interface as a change in state.

When the form is submitted, mutable jumps in and hijacks the interaction,
instead submitting it up the WebSocket. Because Pakyow UI exposes our routes
over the WebSocket just like HTTP, the same restful create route is called,
creating our new comment. Once created, the new comment is pushed down to any
connected client, *including* the client that created the comment.

---
name: Front-End Prototype
desc: Building the front-end of your Pakyow project using view-first development.
---

Pakyow encourages a [view-first development
process](/docs/concepts/view-first-development). This process lets us build a
navigable prototype of our project without writing any back-end code. All it
takes is basic knowledge of HTML to create the front-end presentation layer.

For reference, here's the front-end that we'll be creating:
<br>
<br>
<br>
<img src="/images/warmup-screenshot.png" width="700" alt="Pakyow Warmup Result">
<br>
<br>

The "active viewers" and "all time" counters will tell us how many people are
currently on the page and the number of total views, respectively. These values,
along with new comments, will show up as the state changes on the server. And we
won't have to write a single line of JavaScript!

## Markdown Content

The first part of our view is just some text content. Let's write this in
Markdown rather than HTML. Pakyow makes this easy with view processors. To
install the view processor for Markdown, open `Gemfile` in a text editor and add
this code at the end:

```ruby
gem 'pakyow-markdown'
```

Run `bundle install` at the root of the project and start the server. Now we're
ready to write some content. Create a file named `_content.md` in the
`app/views` directory. Write some markdown content, such as:

```
# Pakyow &ndash; Realtime Web Framework for Ruby

We designed Pakyow to deliver modern realtime features in a traditional backend-driven
architecture. Build websites and apps your users will love, using simple processes
that will keep you smiling. It's unlike anything you've used before.
```

What we've created is a view partial, written in Markdown. Now we can include
the partial into our page. Open up `index.html` and replace the content with the
following HTML markup:

```html
<article>
  <!-- @include content -->
</article>
```

The `@include` directive tells Pakyow to replace the comment with the markup
contained in the `content` partial. Before doing so, the Markdown view processor
converts the Markdown into HTML code.

## Traffic Counters

Next we need to define the front-end for our traffic counters. Add the following
markup between the opening and closing `<article>` tags:

```html
<div data-scope="stats">
  <span data-prop="active">
    1
  </span>

  <span>
    active viewers
  </span>

  <span data-prop="total" class="margin-l">
    2
  </span>

  <span>
    all time
  </span>
</div>
```

## Comment Form / List

Let's create the comment form and list. In a new `app/views/_comment-form.html`
file, add the following markup:

```html
<form data-scope="comment">
  <input data-prop="content" placeholder="your comment here...">
  <input type="submit" value="post">
</form>
```

Next, add the following markup to a new `app/views/_comment-list.html` file:

```html
<p data-scope="comment" data-version="empty">
  no comments :(
</p>

<article data-scope="comment">
  <p data-prop="content">
    Comment Goes Here
  </p>
</article>
```

Finally, include both of these partials into `index.html`. The contents of the
file should now look like this:

```html
<article>
  <!-- @include content -->

  <div data-scope="stats">
    <span data-prop="active">
      1
    </span>

    <span>
      active viewers
    </span>

    <span data-prop="total" class="margin-l">
      2
    </span>

    <span>
      all time
    </span>
  </div>

  <!-- @include comment-form -->
  <!-- @include comment-list -->
</article>
```

## Scopes &amp; Props

You'll notice the `data-scope` and `data-prop` attributes on some of the nodes.
This pattern allows us to label specific nodes as representing the underlying
data of our application.

You can think of a scope as representing a particular data type and a prop
representing an attribute of a type. In the comment list case, the node
containing `data-scope="comment"` represents data of type `post`, and the node
with `data-prop="title"` represents the `title` of a `post`. 

Building in this knowledge of state into the view is a fundamental concept in
Pakyow. We'll see why in the next few sections when we add the back-end code.

## Front-End Wrapup

That's all there is to it! In just a few minutes, we prototyped the front-end of
our project, writing only HTML and Markdown. Next, we'll add the back-end and
render our view with real data.

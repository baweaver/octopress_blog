---
layout: post
title: "Sublime Scoping with Rails"
date: 2015-05-04 22:21:12 -0700
comments: true
categories: [rails, activerecord, controllers]
---

Even the most ardent adherent of skinny controllers will find themselves plagued by the ferocious number of filters demanded for any non-trivial search on their models. Given enough attributes, you'll notice your controller starting to look a little hairy

<!-- more -->

```ruby
class PeopleController
  def index
    @people = Person.where(name: params[:name]) if params[:name]
    @people = @people.where(birthday: params[:birthday_start]..params[:birthday_end]) if params[:birthday_start] && params[:birthday_end]
    @people = @people.where(sex: params[:sex]) if params[:sex]
  end
end
```

The horrifying trend will only continue as our demand for searching power grows, which begs the question: How can we tame this mess?

## Strong Params

Your first line of defense against this will be using strong params to your advantage. They're not only for creating objects.

Let's try something out in the console:
```ruby
[1] pry(main)> ActionController::Parameters.new({a: 1, b: 2})
=> {"a"=>1, "b"=>2}
[2] pry(main)> _.permit(:a)
Unpermitted parameter: b
=> {"a"=>1}
```

So by using permit on our parameters object, we can filter down a hash to only our permitted values. So what if we did something like this?
```ruby
class PeopleController
  def index
    @people = Person.where(params.permit(:name, :sex))
    @people = @people.where(birthday: params[:birthday_start]..params[:birthday_end]) if params[:birthday_start] && params[:birthday_end]
  end
end
```

With that we've already cleaned out a lot of the cruft of our controller, but what about that last one?

## Scoping and Class Methods

We can get rid of it as well, using either scoping or class methods to take care of it for us:
```ruby
class Person
  # We can go with a scope:
  scope :born_between, -> start, end { where(age: start..end) }

  # ...or a class method:
  def self.born_between(start, end)
    where(age: start..end)
  end
end
```

Which will let us trim down our controller even a little more here:
```ruby
class PeopleController
  def index
    @people = Person.where(params.permit(:name, :sex)).born_between(params[:age_start], params[:age_end])
  end
end
```

## Conditional Scoping

The astute reader will note that the above method is going to fail gloriously should we forget either of those params. We could always drop it to another variable and mutate people, but that's generally frowned upon and doesn't normally produce superheroes.

What we can do, however, is introduce a more conditional scoping method. Class methods are, after all, ruby methods. Let's use them to their potential a bit more:

```ruby
class Person
  def self.born_between(start, end = Time.now)
    start ? where(age: start..end) : all
  end
end
```

By throwing in an ``all``, we can conditionally chain freely. 

## Like Scoping

The problem is, that name search just isn't doing it for us. We don't want to break out ``solr`` or ``trigrams`` quite yet, but we can use some ``like`` queries to make it a bit more flexible:

```ruby
class PeopleController
  def index
    @people = Person.where(params.permit(:sex)).born_between(params[:age_start], params[:age_end])
    @people = @people.where('name LIKE ?', params[:name]) if params[:name]
  end
end
```

Though we spend all that time getting rid of postfix ``if`` checks, can we do something about this one as well?

```ruby
class Person
  def self.where_name_like(name)
    name ? where('name LIKE ?', name) : all
  end

  def self.born_between(start, end = Time.now)
    start ? where(age: start..end) : all
  end
end
```

That we can:

```ruby
class PeopleController
  def index
    @people =
      Person
        .where(params.permit(:sex))
        .born_between(params[:age_start], params[:age_end])
        .where_name_like(params[:name])
  end
end
```

Like that, we've eliminated another suffix if.

## More advanced filtering

Though most of these examples have been fairly straightforward, there will be times when you have to break out some joins and other operations depending on your parameters. Strong params aren't going to cut it on those, but class methods just might do the trick.

We have a new model to work with, ``Post``, and with it the following controller:
```ruby
class PostsController
  def index
    @posts = Post.where(params.permit(:name))
    @posts = @posts.join(:users).where(users: {id: params[:user_id]}) if params[:user_id]
    @posts = @posts.includes(:comments) if params[:show_comments]
    @posts = @posts.includes(:tags).where(tag: {name: JSON.parse(params[:tags])}) if params[:tags]
  end
end
```

Some of those earlier techniques just aren't going to cut it, and it's going to be a lot more difficult to be intention revealing here. Including comments and tags unless we have to could be a big expense, so we need to keep those under conditionals to prevent unnecessary data from being fetched.

We're going to have to use something new here. Let's condense those conditionals into a scope:
```ruby
class Post
  def self.by_user(args = {})
    args[:if] ? join(:users).where(users: {id: args[:if]}) : all
  end

  def self.with_comments(args = {})
    args[:if] ? includes(:comments) : all
  end

  def self.with_tags(args = {})
    args[:if] ? includes(:tags).where(tag: {name: JSON.parse(args[:if])}) : all
  end
end
```

Noted that keyword arguments would be very unhappy with us using ``if`` there, making it a no-go.

Which allows us to write a much clearer controller:
```ruby
class PostsController
  def index
    @posts =
      Post
        .where(params.permit(:name))
        .by_user(if: params[:user_id])
        .with_comments(if: params[:show_comments])
        .with_tags(if: params[:tags])
  end
end
```

Through just a few simple scoping mechanisms, we can trim down our controllers while still getting a very useful search from vanilla rails.

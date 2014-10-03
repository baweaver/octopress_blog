---
layout: post
title: "Many Woes with Many to Many Relations"
date: 2014-10-01 21:11:59 -0700
comments: true
categories: [Ruby, Rails, Routes, AngularJS, API, ActiveRecord]
---

Rails provides us with a lot of power in routing and associations, but if you've ever tried to set up an API with any form of many-to-many relationship, you're in for a nightmare. Google won't save you, the Rails guides are sparse, and there's a grand total of [one good blog post](http://ngauthier.com/2010/11/restful-many-to-many-relationships-in-rails.html) on the matter from a few years ago.

<!-- more -->

## Many to Many

So how does a many to many relationship work? Via an association table containing IDs of both of the resources to be linked. Both then have access to the other collection. It's extremely handy for certain problems, and if you're just using Rails through the view you'll likely never have a problem with it.

## The Fun Starts

But now you've heard about this awesome thing called Angular / Ember / New Hot JS Framework that you just have to use. I don't blame you, a few weeks in Angular and I don't want to use Rails Views again. You decide to take the high road and segregate the apps, making Rails an API and using your framework (Angular assumed from here on out) to build out the frontend through calls.

It all works great, you even found [RestAngular](https://github.com/mgonto/restangular) to help you out with some of the plumbing. Simple actions are now trivial. Want a list of a Users comments? Easy:

```javascript
// Livescript
RestAngular.one \users, 1 .getList \comments .then (data) -> $scope.comments = data
```

## But then there are Categories

RestAngular already has us covered, any other case and we're sailing along. Now we want to add categories to our posts, a many to many relationship. How would we script that one? Likely the first thing you try is this:

```javascript
// Livescript
RestAngular.one \posts, 1 .getList \categories .post formData
```

Checking the DB, you'll notice the new association isn't there. Odd. Maybe delete will work?

```javascript
// Livescript
RestAngular.one \posts, 1 .one \categories, 1 .remove!
``` 

...except now for some reason, category one is gone everywhere. Thinking through it, it becomes clear that what we've done is simply request a nested resource and sent it a delete request.

## So what do you do?

There's an association table with your name on it called something like PostCategory. Trying to route through either one of the hosts is likely to give you nightmares.

First let's take a look at what your controller action should look like to handle the queries:

```ruby
def index
  PostCategory.where(params.slice(:post_id, :category_id))
end

def create
  @post_category = PostCategory.new(post_category_params)

  if @post_category.save
    render json: @post_category, status: :createds
  else
    render json: @post_category.errors, status: :unprocessable_entity
  end
end

def destroy
  if params[:id]
    PostCategory.find(params[:id]).destroy
  else
    PostCategory.where(post_id: params[:post_id], category_id: params[:category_id]).first.destroy
  end

  head :no_content
end
```

Note that the index action is a very succinct way of saying:

```ruby
def index
  post_categories = PostCategory.all
  post_categories = post_categories.where(post_id: params[:post_id]) if params[:post_id]
  post_categories = post_categories.where(category_id: params[:category_id]) if params[:category_id]
end
```

Though the latter has been known to drive me to very lengthy discussions on mutability morality and ethics.

This allows us to search against either posts or categories depending on the params, but this can only work if we cheat a bit around the routes and define a ``DELETE`` action on the root resource:

```ruby
delete '/post_categories' => 'post_categories#destroy'
```

Not exactly the most straightforward method, but given the odd alternatives like adding controller actions to either of the ends of the relation like ``post#add_category`` and adding multiple routes for every time you try it I far and prefer this idea. The only real difference is that you end up with a request like this instead:

```
DELETE mysite.com/post_categories?post_id=1&category_id=1
```

Now all we have to do are basic actions like on any other service and we're golden:

```javascript
// Livescript
RestAngular.all \post_categories .post
  post_category:
    post_id: $scope.new_category.post_id
    category_id: $scope.new_category.category_id

RestAngular.all \post_categories .remove
  post_id: $scope.new_category.post_id
  category_id: $scope.new_category.category_id
```

Wrap it in a service and you're set to go. Just remember that the association tables are there for a reason, use them. Rely on too much rails magic and you'll end up burned thinking something's going to work.

I welcome any thoughts on how better to address such issues as this in the comments, I'd love to hear your opinions!

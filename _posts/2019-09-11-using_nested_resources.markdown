---
layout: post
title:      "Using Nested Resources"
date:       2019-09-11 17:40:29 +0000
permalink:  using_nested_resources
---


I decided to write this blog about nested resource routes in an effort to help me better understand what I should be learning in this section. 

What I have found about rails is that it is supposed to be "magical" but while going through this process, I have felt everything but magic .  Mainly because Rails takes care of A LOT behind the scenes of the code you actually have to write. Which is helpful, but leads to minor confusion for those individuals new to coding like myself. 

Nested resources are used for when a route exists only for a specific view. For example, a review on a restaurant. The review does not exist for every restaurant because maybe that user has not visited every restaurant in our database. (Also it  wouldn't make sense for a user to suggest a bacon cheeseburger in their review of a vegan restaurant.) In order for us to make this distinguishable to the program we are using, we rely on nested resource routes. 

To clarify, here are some code snippets of what this would look like in Ruby:

`GET /restaurants/:restaurant_id/reviews` <- This would show  all of the reviews for this specific restaurant based upon its given ID.


`POST /restaurants/:restaurant_id/reviews` <- By changing the HTTP verb, we are now creating a review about that restaurant

Now, let's say we're posting the review from our phone and accidentally hit complete review before we are done

Our review will be given an ID which would then create a route like this: `/restaurants/:restaurant_id/reviews/:id` 
A simple change of the http verb from "POST" to "PUT" will now create a route for us to update that review.
And if we wanted to delete that comment, it would be very simple, we would just change the HTTP verb again to....you guessed it, "DELETE" 

The reason these are called nested resources is because in this example, the reviews are "nested" within each restaurant resource. 

Rails has an idea of convention over configuration. so therefore, when you write `resources :restaurants` rails assumes you will be using a restaurants controller making it easier on the developer when it comes to configuring routes. 

With that knowledge we can take this further by characterizing what we want our users to be able to access within those resources. 

If a restaurant is created and I do not want my users to delete that restaurant, I could do this two ways by using  `:only` or `:except` in this case we would use the latter like this: `resources :restaurants, :except => [:delete]`

When we nest our resource within another, we write it like this: 
`resources :restaurants do
          resources :reviews 
end`       

This allows us to access all of the reviews through the routes listed above, as well as add new reviews. 

If we want to update or destroy a review the routes are reduced to only `/reviews/:id` because once you have the :id for the nested resources, we don't need the beginning part. All we need is to specify our verb.  In order to accomplish that and work on the idea of separating our concerns we could use `:except=> [:update, :destroy]`. 

After doing that we could call a separate `resources :reviews, :only => [:update, :destroy]` .



That pretty much wraps up what I know about nested resources.

Thanks for reading!

---
layout: post
title:      "The Scoop on Scope"
date:       2019-12-15 11:13:38 -0500
permalink:  the_scoop_on_scope
---


The idea of blogging about Ruby Scope came to be because of my Flatiron School Rails project. Which, I guess it wouldn't hurt to give you the lowdown on that actual project; considering this blog is a part of this task.

## CSI Product Review Program
I would hope that the title of the application gives away the objective. But if not, here’s the low-down:

* A user visits the site
* That same user can sign up, create a new login or login using their Google credentials
* Once logged in, the user can add products to the application, review already existing products, or they are free to edit or delete their own products.
* Users can view other user profiles, as long as they are logged in.
* Anyone can visit the application site and look at products and reviews but can not leave reviews or manipulate any products, chemical groups or application areas without being logged in and, even more so, they must be the owner of the product or profile. 
* Also, no duplicate product reviews! That’s cheating. No review inflation allowed here!
* The page layouts include categorizing products based on their review score, as well as filtering users based on the number of reviews they have left for products.

## What is Scope?
Part of the requirements for the project was to include a class level ActiveRecord scope method. We haven't touched on this in a good while so I thought it would be best to have a refresher.

### Definition

> **scope(name, body, &block)** - A scope represents a narrowing of a database query. The method is intended to return an ActiveRecord::Relation object. - [api.rubyonrails.org](https://api.rubyonrails.org/classes/ActiveRecord/Scoping/Named/ClassMethods.html)

In other words, I need to take the information that my application is collecting and find a way to narrow down relationships between those objects, and possibly only narrow down within a specific instance. Scoping is not limited to the bigger picture of between tables (objects). You could also create a scope method for one class, which I will give an example of later in this blog.

## Scope model method - changing the view of the collection

With my application, I wanted to show the user a correlation between products and reviews.

> *SIDENOTE:* Reviews are "nested" within products. Why? Because the user needs to review *something*. You can't just have a general review floating around on a website with no tangeable attachment.

I thought it would be a good idea to include an average review score for each product. And, if the product did not have any reviews, include a note stating that the product had no reviews. I didn't just want a total count of reviews. I wanted a similar concept as you would see on Amazon - an average of a review score for each sold item.

### Where to begin?

Good question... Well, considering that products and reviews are on completely different tables, I needed to come up with a way to pull both a product and all reviews for that one particular product, then get an average for a review score (or, "rating" in this case).

This involves "joining" the tables within my scope - and this is where it can get tricky. Which "join" should I use?

Well, I didn't want to just "match" my products and reviews (join table). After all, my project setup is a product `has_many` reviews and a review `belongs_to` a product.

### Create the relation

In order to show ALL my products and ONLY show the corresponding reviews, I went with a *left join* setup:

```
//the syntax
scope: name, -> {lambda syntax body}
```

```
//my code
scope :order_by_rating, -> {left_joins(:reviews).group(:id).order('avg(rating) desc')}
```

Just to clarify, the above code is on the `Product` model. My above code is asking the database:

> Go to my database tables and grab the reviews and group by id, then order those in descending order based on the average rating. 

The `left_joins` statement tells the query that I still want to show ALL my products, but to only join and show those matching reviews for that particular product on the product index page.

The "product index page" part is physically setup on the product index page. Nowhere in the above code do I specify to "show" this on the index page.

### Clean up the view

I then created a `class scope method` that would sort my products by average rating. Again, this is in the `Product` model:

```
def average_rating
    if self.reviews.size > 0
      self.reviews.average(:rating)
    else
      'no reviews'
    end
  end
```

The above code is telling my model:

> If `product.reviews` has a size (or review count) greater than 0, average those reviews by rating and dislpay to viewer. If the `product` has 0 reviews, then display the message "no reviews"


### Putting it all together

First, I would need to call the scope method in the `Products Controller`

```
def index
        @products = Product.order_by_rating
    end
```

This manipulates the `@products` instance and calls the scope method on all of the products.

Now, to create a better "visual" for the viewer in my products index page I call on the `avarage_rating` scope method within the view:

```
// some code
<% @products.each do |product| %>
    <h2><%= link_to product.name, product_path(product) %> - Average Rating: <%= product.average_rating %></h2>
// finishing code
```

## Scoping Even Further

To take the whole "scoping" concept a little further, I also wanted a way for my viewer to ONLY see the product with the most reviews. This again would involve querying the database and creating a relationship between tables (models).

### Create the relation

Ok, how do I **ONLY** show the product with the most reviews? Well, I wouldn't do a `join_left` because that would show ALL products with the reviews "joining" over to the product table.

If I use `join` instead, then I would be asking the database to take the `products` table and the `reviews` table and **ONLY MATCH** the correlations, meaning:

> Only show the viewer products that have reviews. Hence, the **MATCH**

So, this time I would do the following on my `product` model:

```
scope :most_reviews, -> {joins(:reviews).group('reviews.product_id').order("count(reviews.product_id) desc").limit(1)}
```

I give my scope method a name, then in the body I tell my database:

> Group together the `reviews` table with the `products` table by the `reviews.product_id` and order the result by the total count of `reviews.product_id` in descending order. Also, limit the result by one

Meaning, only show the result of *one* product with the most reviews.

### Relation on display

Now, I don't want to just *throw* this result on the same page the viewer is currently viewing. When the viewer clicks on the link to see the product with the most reviews, I want to send that viewer to a new page to display that product with the largest review count.

I'll need to create a route for this page:

```
get 'products/most_reviews' => 'products#most_reviews'
```

Now, I'll need to go into my `Products Controller` and create the needed route to connect to a view page:

```
def most_reviews
        @product = Product.most_reviews.first
    end
```

This is calling the scope method from the `Product` model onto the `@product` instance.

Now, in my view, I code the following:

```
<h1><%= @product.name %> has the most reviews</h1>

<h3>Review Count: <%= @product.reviews.count %></h3>
```

Because I'm calling scope method, `most_reviews`, on the `@product` instance, the view will result as such.

Also, because I created the product/review table relation using `join`, I can now reference `reviews` within my view, hence the `<h1><%= @product.name %> has the most reviews</h1>`

## Back to the project at hand

Feel free to watch a walk-through of the finished application project:

<iframe width="560" height="315" src="https://www.youtube.com/embed/lmRC-Gzh6-s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

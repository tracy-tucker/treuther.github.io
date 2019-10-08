---
layout: post
title:      "COME BACK TO THE TITLE"
date:       2019-10-08 22:44:03 +0000
permalink:  come_back_to_the_title
---


Before I start, I feel like I need to back-track a bit. I need to rewind the tape back to my first lab, the CLI Gem project. When I tackled this, I honestly didn't know what I was doing. Ha! There, I said it! I do feel better after admitting that out loud. Anyway, I felt very lost in how to use the Ruby programming language eventhough I spent a great deal of time completing lessons leading up to that very first project.

Not to say I'm a programming "genius" at this point, but I do feel better about confidently not knowing - is that a thing?

My intentions for this project was to fill a need at my current place of employment. After all, that's the real-world, right? As programmers, we create solutions that fit the needs of the client. Not necessarily a solution that we may personally need.

I chose to develop an application that allows users to add and track insecticide products that pest management professionals use to control insects. I know, it doesn't sound that glamorous but there is a need for this app where I currently work. So, after I graduate, I will be coming back to this project and ellaborating in areas that could better fit the need of the company.

## Where Do We Go From Here?

Before I could even begin putting finger-to-keyboard, I needed to kick it old school (pencil and paper) and figure out what tables I wanted, what columns would be needed, and how those tables would relate with one another.

To me, this is the most crucial part of the application. This is the section you need to think long and hard because this sets the stage for your database and how those tables will work together.

![](https://res.cloudinary.com/tracyr/image/upload/c_scale,w_500/v1570574513/20191008_173702_xfhrwc.jpg)

### USERS

The USER table is the top-dog. Why? Because the premise of this lab is to have a USER create an account to create and modify THEIR OWN products. The USER needs a `username`, `email address` and` password`. In this case, I am marking this as `password_digest` because this is needed for the Bcrypt gem.

Bcrypt protects your users against hackers. When a user creates a password, Bcrypt uses a hashing algorithm to create a "digital footprint" ( a random series of numbers and letters) of the user's password, which cannot easily be reversed.

For the relationship setup, USER `has_many` PRODUCTS. Because, again, the USER is the top-dog table who will own and control their created products. The USER table also has the relationshp, `has_many` BUGS `through` PRODUCTS. We will get to that in a moment.

### PRODUCTS

The PRODUCTS table is essentially the data that the USER will be creating. All PRODUCTS will have a `product_name`, `active_ingredient`, `description` and `user_id`. The USER_ID is the FOREIGN KEY. This connects PRODUCTS to a USER, thus the relationship PRODUCT `belongs_to` USER.  PRODUCTS also has the relationship `has_many` PRODUCTBUGS and `has_many` BUGS `through` PRODUCTBUGS. This is where JOIN TABLES come in. We will touch on this in the BUGS and PRODUCTBUGS section.

### BUGS

The BUGS table is the scraggler table. Why? The best analogy I can think of comes from one of the lab lesson on songs, artists and genres:

> One Artist can create multiple songs that can be categorized by different genres. The Genres don't belong to the Artist, but the songs do. But genres are still there because of the songs.

I honestly don't know a better way to explain that. In my head it makes sense, but sharing that with others is a little tricky.

The reason why I created a BUGS table is because I wanted a USER to be able to create a PRODUCT that could later be viewed (or categorized) by the type of BUG the PRODUCT helps control. So, BUGS have a `bug_name`, and have the relationship `has_many` PRODUCTBUGS, `has_many` PRODUCTS `through` PRODUCTBUGS, and `has_many` USERS `through` PRODUCTS.

### PRODUCTBUGS

This is our JOIN TABLE.  This is a necessary table because BUGS do not *directly* relate to USERS - only *indirectly* through PRODUCTS. This table right here allows for BUGS to be associated with a USER and a PRODUCT at the same time. Because it is a JOIN TABLE, it will only utilize foreign keys to create the indirect relationships. PRODUCTBUGS will have a `product_id` column and a `bug_id` column. Both are being used as foreign keys to PRODUCTBUGS.

Honestly, creating the appropriate tables is the hardest part. My best advice is to find an analogy that best helps you understand the different relationships and compare that to what you are trying to accomplish with your new tables.

## Project Skeleton Setup

This project was developed using the Sinatra and ActiveRecord Ruby libraries. Sinatra is a web application framework that is Rack based with pre-written methods, such as GET and POST. These allow your application to respond to HTTP requests. ActiveRecord is a Ruby library that gives the developer the ability to work with relational SQL databases by providing Object Relational Mapping (ORM). This allows the developer to interact directly with an object using the same language they are currently coding with.

ActiveRecord offers these core features:

* a single Ruby object maps to a database table
* columns are accessed by methods, and are inferred from the database schema
* methods for create, read, update and destroy (CRUD) are defined
* a domain specific language (DSL) for easily constructing sequel queries in Ruby

I installed the gem, Corneal, which is "a Sinatra app generator with Rails-like simplicity". I can't take any credit for that line. That's straight off of the gem readme. Essentially, this gem set up everyting I needed in order to create my database and associating tables, my Models, Views and Controllers (MVC) and the presets in my controller for CRUD (create, read, update and destroy).

From here on out, it's literally using the Ruby language to create the appropriate CRUD methods within the appropriate controllers. With these we are able to make HTTP requests and output the appropriate results to the viewer.



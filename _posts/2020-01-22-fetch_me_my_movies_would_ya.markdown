---
layout: post
title:      "Fetch me my movies, would ya?"
date:       2020-01-23 00:18:07 +0000
permalink:  fetch_me_my_movies_would_ya
---


I really struggled with this Flatiron School JavaScript project. More specifically, *how* to put all the pieces together and fit where they are suppose to go but also to get them to *function* as a whole.

My grandmother LOVED movies. She would watch at least 1 to 2 movies almost every day of her life. She lived to be 81 so one could say she could have been a screenwriter or a director herself! Anyway, she would keep record or every single movie she ever watched. She had notebooks upon notebooks full of the movie title, the day she watched the movie and if it was any good. Before renting a new movie, she would check her notebook to make sure she didn't get a repeat movie, although I did hear her say a couple of times that she had accidentally grabbed a "repeater".

I decided to create an application that carries on the legacy of my grandmother. The application allows the user to keep record of every movie they ever watched. Of course, my "movie recorder" is a tad different from my grandma's but if she were still alive she would have loved it! It would have made her life much easier versus referring to piles of notebooks before renting a movie.

## The Movie Doc App - General Overview

Not a very clever name, but I can work on that later. Essentially, the single page application includes a Rails API for the backend that demonstrates a client-server communication with a frontend made up of HTML, CSS and JavaScript. The frontend heavily relies on Object Oriented JavaScript in order to grab my seed data from the backend and display that to the frontend. It is built to be a simple, user friendly movie record application, but also built in such a way for additional features to be added in the future.

Right now, the application allows the user to record a title, genre, year of movie release, the parental guidance rating and a brief description of the movie plot. The functionality allows for a user to enter all movie input requirements, save the movie without reloading the page (AJAX), double-click movie inputs for editing and "click away" for instant save without page reload (AJAX). Additional features include sorting by title, genre or year of release.

### The Backend

Movie `belongs_to` a genre and a genre `has_many` movies for the model relationships. Movie has a basic CRUD controller setup, with the only difference of using FAST JSON serializer. It provides a way to generate serializer classes for each resource object in an API that is involved in customized JSON rendering. In other words, instead of writing a custom render each time, I would only need to write out a serializer for each object once and then use FAST JSON API to control the way the data is structured.

I forgot to mention I also installed the Rack CORS gem. This install creates a file in environments, inside the initializers folder. The acronym stands for CROSS-ORIGIN RESOURCE SHARING. CORS allows a web app to make cross-domain AJAX calls without needing to use workarounds. For the sake of this project, I set  origins to `*` to allow any AJAX calls.

### The Frontend

#### HTML

As mentioned above, the frontend is a conglomerate of HTML, CSS and JS. I kept my HTML very basic, but probably a little more HTML heavy on the form compared to other student JS projects.

####  CSS

I added a little bit of styling to the frontend. Just enough to give some color to stylize the form and to visually separate line items. I focused on `input attributes` for styling specific line items vs others.

#### JS

I have a few different folders set up in order to keep my JS files separate (or modular) depending on the purpose of the code.

The `index.js` file is the "kick-off" file to create a new app componenet. The `app.js` file has an app class that creates a new instance of Movie.

The purpose of the  `MoviesAdapter.js` file is to do all the communicating with the backend API. Variables are set through the constructor, which then passes the value of that variable on down to the methods that are created within the MoviesAdapter class.

As mentioned previously, the purpose of the MoviesAdapter is to communicate with the backend API. With that being said, all methods within the MoviesAdapter serve the purpose of backend communication, such as the methods, `getMovies`, `createMovies`, `updateMovies` and `deleteMovies`.

Each method utilizes the `fetch()` method. Why? Because the `fetch()` method has several built-in technologies that AJAX relies on to make requests to the server without reloading the page

The `fetch()` method takes in one mandatory argument - the resource you want to fetch (JSON data). `fetch()` then returns a `promise` (rit epresents the completion of asynchronous operation) that resolves to the `response` to that request. Each response is then "handled" with `.json` to convert the response into JSON. The second `.then` is used  for DOM manipulation.

Now, the `createMovies`, `updateMovies` and `deleteMovies` pass in a second argument for `fetch()` These headers are required because they represent the actions and methods of a form that tells the backend the *type* of information being passed in, and *how* to handle that information once passed over (IE create it, delete it).

#### The Meat - Movies.js

To me, I feel like the this is the "meat" of the application. Yes, all components are important, but this is the JS file that communicates between the HTML frontend and the MoviesAdapter which communicates directly with the backend. It is structured using Object Oriented JavaScript by separating code into a collection of cells. This keeps all functions very organized based on their purpose, allows for better control by encapsulating data, and makes it much easier to create objects that inherit all set variables and functions (or in this case, methods) based on the specific class the object is created by.

The `Movies` class constructor has the basics of an empty array, an argument that assigns `movie.adapter` to an instance of a new MovieAdapter, and invocations to initialize 2 important functions (AKA methods when inside of a JS class).

One of those methods is the `initBindingAndEventListeners`. This method contains a group of JS statements with the common purpose of grabbing elements from the frontend  and assigning those to variables to be utilized in methods within the `Movies` class. It also contains the creation and initialization of event listeners that `bind` the event listener function argument to the Movie. The `bind` is essentially a copy of the function that sets the execution to the argument that's passed in to `bind`.

All other methods within the `Movies` class have the purpose of manipulating DOM elements in order to "control" what the user experiences when performing certain tasks in the browser, as well as communicating that manipulation with the MoviesAdapter if that manipulation involves retrieving, creating, updating or deleting data from the server.

#### Movie.js

This file contains a `Movie` class that handles the rendering of each individual attribute of a movie, which is seen in the constructor. The method `renderLi()` is part of ANOTHER method within `Movies.js` that sets the `innerHTML` of the `moviesContainer` div to the iterated data chosen from the JSON structure.

Whew, that sounds confusing!


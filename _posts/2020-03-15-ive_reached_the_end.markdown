---
layout: post
title:      "Movie Emporium"
date:       2020-03-15 17:46:57 -0400
permalink:  ive_reached_the_end
---

## A React Application for Tracking Movies

With everything going on in the world, AKA COVID-19, this app couldn't have had better timing. I'm sure plenty of people will be watching plenty of movies, which means there's a chance that one could unintentionally rewatch a movie because *they didn't have a cool way to keep record of already watched movies*... just sayin'.

## Backend - Rails API
The "Back-side" of the application was developed using a Rails API.

### File Structure

Here is an abbreviated snapshot of the backend structure:

```
#Genre.rb
has_many :movies
validates :name, presence: true

#Movie.rb
belongs_to :genre

validates :title, :rating, :description, presence: true
validates :title, uniqueness: true
validates_inclusion_of :rating, :in => ["R", "PG-13", "PG"]
```

With this, I also initialized Serializers to define the basic JSON:API resource objects.

Naturally, with this setup, I would have at least 2 routes. I nested `movies` under `genres`, as well as created a separate route for movies to give users an option to see a quick list without having to navigate through each genre.

```
#...

namespace :api do
    namespace :v1 do
      resources :movies
      resources :genres do
        resources :movies
				
#...
```

My CRUD actions for each controller are complete enough for the requirements of the project. `Genre` does not have a delete action and `Movie` does not have an update action. I do intend on adding a movie update action, which may or may not be complete before the review.

## Frontend - React-Redux

I ended up with quite a few components, although I'm sure the number of components I'm using doesn't even compare to the other projects - or a real-life project, for that matter!

### Index.js

This file initializes the communication between the React library and Redux state container. I also implemented the following:

```
//...

import {createStore, combineReducers, applyMiddleware, compose} from 'redux'; 
import thunk from 'redux-thunk';   //allows you to write action creators that return a function instead of an action.
import {Provider} from 'react-redux';   //makes the redux store available to any nested components.
import {BrowserRouter as Router} from 'react-router-dom';   //matches a branch of its routes, and renders their configured components. 

//..
```

### combineReducer

I created a `combineReducer` helper function for index.js. A `combineReducer` helps turn an object whose values are different reducing functions into a single reducing function you can pass to `createStore`. This was necessary because `createStore` only allows ONE reducer to be passed in. Eventhough movie belongs_to genre, I still have movies coming in using their own route. Like I said, not necessary for such a small application, but this helped me better organize the state tree and to access genre and movie from their own tree "branch". 

```
//...

const reducer = combineReducers({
    genres: genreReducer,
    movies: movieReducer
})

//store = globally storing data
let store = createStore(reducer, composeEnhancers(applyMiddleware(thunk)))

//...
```

This allows for each reducing function to independently manage parts of the  state. This comes in handy for much larger projects. I went ahead and used in this project because I am so new to React-Redux and this helper function actually helped me better understand and manage the flow of state.

```
//...

ReactDOM.render(
    <Provider store={store}>
        <Router>
            <App />
        </Router>
    </Provider>
    ,
document.getElementById('root'));

//...
```

The reducer that is passed to createStore then gets passed to App.js with the help of `Provider`. `Provider` makes the Redux `store` available to any nested components that have been wrapped in the `connect()` function.

### App.js
My `App.js` file is passing all the main components to display to the user. It's basically "housing" the entire application. Instead of pulling the MoviesContainer directly, I am using a Route with exact path to access any movie components. This is also where I have a route to NavBar, as well as a route to the home page.

### Containers
I only have 2 containers - `GenresContainer` and `MoviesContainer`. The purpose of a container is to fetch data and render corresponding subcomponents. Both containers are fetching data from the store and utilizing `mapStateToProps` to pass that `state` as `props` to the corresponding subcomponents.

```
//GenresContainer

//...

const mapStateToProps = state => {
    return {
        genres: state.genres.genres
    }
}

export default connect(mapStateToProps, {fetchGenres})(GenresContainer);


//MoviesContainer
//...

const mapStateToProps = (state, ownProp) => {
    const movieGenre = ownProp.genre

    if(movieGenre) {
        return {
            movies: state.movies.movies.filter(movie => movie.genre_id == movieGenre.id)
        }
    } else {
        return {movies: state.movies.movies}
    }  
}

export default connect(mapStateToProps, {fetchMovies})(MoviesContainer);

```


There is a conditional included in the `mapStateToProps` in the `MoviesContainer` because there are 2 ways a user can pull movies - go through each individual Genre to see movies, or directly see a list of movies. The user's path would determine which state gets utilized.

`ownProps` is an optional second argument that `mapStateToProps` can pass in that can return a plain object containing the data that the connected component needs - in this case, the `genre` state. the `Genre.js` component is passing in the `genre` state automatically to the `MoviesContainer` because `Genre.js` is "calling", or rendering, the `MoviesContainer`.

### Actions and Reducers
Redux Actions are payloads of information (and the only source of info for the store) that send data from the application to the store.

I have actions to get, add, edit and delete for Genre and Movie, each using asynchronous functions. Those functions pass the JSON-parsed promise to the corresponding reducer that then specifies how the application's state changes in response to actions sent to the store.

### Components

I have both class and functional components. Components that are involved with updating the store are built as class components because these are taking in new information to be passed to the store. All other components are built as functional components becase these are simply taking state as props to display that info to the user.

There is so much more that can be written to explain each component used to build this application. And, quite frankly, this post could be split into a topic cluster of a few sub-blogs just to convey the entire message. Instead this is a lengthy overview of the project as a whole.

## Video
I do have a video that gives a brief demonstration of the demo project in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/HzdPbQ9Yb00" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

There is plenty of room to add more to this project, which I do plan to do on my own.

Signing off...

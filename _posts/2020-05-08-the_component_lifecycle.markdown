---
layout: post
title:      "The Component Lifecycle"
date:       2020-05-08 18:06:20 +0000
permalink:  the_component_lifecycle
---


React lets you define either class components or functional components. Each component has a lifecycle process with several lifecycle methods.

### Functional Components
Functional components are considered "stateless", or "idiot" components because it is just a plain javascript function that takes props as an argument and returns a react element.

As far as behavior goes, Functional components return HTML, and behave pretty much the same way as a Class component.

```
function Welcome(props) {
     return <h1>Hello, {props.name}</h1>;
		 }
```

### Class Components
Class components are packed with far more features and lifecycle methods. These can be called "stateful" or "smart" components by developers. Class components have a state, lifecycle methods and is a JS class.

```
class ClassComponent extends Component {
     render() {
		      return <h1>I'm a class component.</h1>
					}
}
```

## Mounting, Updating and Unmounting

![](https://res.cloudinary.com/tracyr/image/upload/c_scale,w_800/v1588353294/Screen_Shot_2020-05-01_at_12.08.05_PM_srnqdn.png)

Each component has a lifecycle process of Mounting, Updating and Unmounting. Some would argue there is also a "Premounting" process. From what I've read, there are some who recognize this phase and others who lump all "Mounting" methods within the Mounting phase. Each of these phases can be monitored and manipulated.

Both Mounting and Updating blend between the "render" phase and the "commit" phase, while Unmounting is only within the "commit" phase.

### Mounting

The following methods are called, in this order, when an instance of a component is being created and inserted into the DOM:

1. `constructor()`
2. `getDerivedStateFromProps()`
3. `render()`
4. `componentDidMount()`

The render() method is required with class components and will always be called. The others are optional and will be called as they are defined.

#### CONSTRUCTOR()

This method is called BEFORE anything else when the component is initiated (Pre-Mounting...). This is also where you setup the initial `state` and other initial values. You should always use `super(props)` in order to allow components to inhereit methods from its parent. Typically, React constructor() is only used for 2 purposes:

* Initializing local state by assigning an object to `this.state`
* Binding event handlers methods to an instance

#### getDerivedStateFromProps()

This method is called right before the elements render in the DOM, both on initial mount and any updates thereafter.

This is a rarely in cases when state depends on changes in props overtime, such as a transition component that compares previous state and next children.

#### render()

As mentioned, `render()` is the only required method for a class component. It will inspect `this.props` and `this.state` and return a type:

* React elements
* Arrays and fragments
* Portals
* String and numbers
* Booleans or null

Render should provide a PURE rendering and not modify a component state.

#### componentDidMount()

This method is invoked immediately after a component is mounted. This is where you would run statements that require that the component is already in the DOM. An example of this would be a "subscribe" option.

This is also a good place to call `setState()`. This will cause an additional rendering, but this happens before the browser updates.

```
componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
```

### Updating

The next phase in the lifecycle process is Updating. This is triggered when there is a change in the component's states or props.

Here are the five methods in order:

1. `getDerivedStateFromProps()`
2. `shouldComponentUpdate()`
3. `render()`
4. `getSnapshotBeforeUpdate()`
5. `componentDidUpdate()`

#### getDerivedStateFromProps()

This is the first method that gets called when a component gets updated. Again, this is used in rare cases.

```
...
constructor(props) {
  super(props);
	...
	
	static getDerivedStateFromProps(props, state) {
	  return { favoritecolor: props.favcol; }
}
...
```

#### shouldComponentUpdate()

This method is used to return a Boolean that will assess whether or not the component should continue to render or not.

The default value is `true` unless specified by defining the function.

#### render()

As with Mounting, `render()` is required. It will be called when a component is updated and render the new changes to the DOM.

#### getSnapshotBeforeUpdate()

This method has access to props and state BEFORE the update. In order to utilize this method, you also have to use `componentDidUpdate()`, otherwise you see an error message. `componentDidUpdate()` is needed because any information captured in `getSnapshotBeforeUpdate()` is then passed as a parameter to `componentDidUpdate`

A common example found online for use of this method would be to utilize it to capture scroll position before an update.

```
getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
  }
  componentDidUpdate() {
    document.getElementById("div2").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
```

#### componentDidUpdate()

Of course, this method is called AFTER the component is updated in the DOM.

```
componentDidUpdate() {
    document.getElementById("mydiv").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
```

Also, this is a good place to do network requests, as long as you are comparing current props to previous props.

```
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```

### Unmounting

The final lifecycle process is triggered when a component is removed from the DOM. There is only one built-in method for this:

* `componentWillUnmount()`

#### componentWillUnmount()

This method is called when a component is ABOUT to be unmounted.

```
import React from 'react';
import ReactDOM from 'react-dom';

class Container extends React.Component {
  constructor(props) {
    super(props);
    this.state = {show: true};
  }
  delHeader = () => {
    this.setState({show: false});
  }
  render() {
    let myheader;
    if (this.state.show) {
      myheader = <Child />;
    };
    return (
      <div>
      {myheader}
      <button type="button" onClick={this.delHeader}>Delete Header</button>
      </div>
    );
  }
}

class Child extends React.Component {
  componentWillUnmount() {
    alert("The component named Header is about to be unmounted.");
  }
  render() {
    return (
      <h1>Hello World!</h1>
    );
  }
}

ReactDOM.render(<Container />, document.getElementById('root'));
```

## Why No Arrow Functions in Lifecycle Methods

An arrow function does NOT have its own `this`, rather, it does not have its prototype property. Instead, the `this` value of the enclosing lexical scope is used. So, for example, an arrow function will go searching for `this` which is not present in the current scope, so instead the arrow function will find `this` in its enclosing scope.

EXAMPLE:

```
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the Person object
  }, 1000);
}

var p = new Person();
```

### So, why not use arrow functions in lifecycle methods???

Lifecycle methods written using arrow functions will not exist on the prototype chain of whatever JavaScript class you've created. The prototype chain ends when it reaches null. Lifecycle methods are already implicitly bound to the class through props and state.

## What Are Hooks?

Hooks are called inside a function component to add some local state to it. They let you "hook into" React state and lifecycle features. This was introduced in React 16.8 and is backwards-compatible. I would understand this as a means to have a local state VS trying change your entire component to be a class component.

```
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```






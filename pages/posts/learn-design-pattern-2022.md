---
title: Design Patterns - 2022
description: Design Patterns - 2022
date: 2022-03-01T00:00:00.000+00:00
lang: en
type: project
duration: 2months
---

[[toc]]

## Introduction

Design patterns are a fundamental part of software development, as they provide typical solutions to commonly recurring problems in software design. Rather than providing specific pieces of software, design patterns are merely concepts that can be used to handle recurring themes in an optimized way. Over the past couple of years, the web development ecosystem has changed rapidly. Whereas some well-known design patterns may simply not be as valuable as they used to be, others have evolved to solve modern problems with the latest technologies.

## Singleton Pattern

```txt
Share a single global instance throughout our application
```

Singletons are classes which can be instantiated once, and can be accessed globally. This **single** instance can be shared throughout our application, which makes Singletons great for managing global state in an application.

First, we’re going to build a **Counter** class that has:

- a `getInstance` method that returns the value of the instance
- a `getCount` method that returns the current value of the counter variable
- an `increment` method that increments the value of counter by one
- a `decrement` method that decrements the value of counter by one

```js
let counter = 0;

class Counter {
  getInstance() {
    return this;
  }

  getCount() {
    return counter;
  }

  increment() {
    return ++counter;
  }

  decrement() {
    return --counter;
  }
}
```

However, this class doesn’t meet the criteria for a Singleton! A Singleton should only be able to get instantiated **once**. One way to make sure that only one instance can be created, is by creating a variable called **instance**. In the constructor of Counter, we can set instance equal to a reference to the instance when a new instance is created. We can prevent new instantiations by checking if the instance variable already had a value. If that's the case, an instance already exists.

```js
let instance;
let counter = 0;

class Counter {
  constructor() {
    if (instance) {
      throw new Error("You can only create one instance!");
    }
    instance = this;
  }

  getInstance() {
    return this;
  }

  getCount() {
    return counter;
  }

  increment() {
    return ++counter;
  }

  decrement() {
    return --counter;
  }
}

const counter1 = new Counter();
const counter2 = new Counter();
// Error: You can only create one instance!
```

Perfect! We aren't able to create multiple instances anymore.

### (Dis)advantages

Restricting the instantiation to just one instance could potentially save a lot of memory space. Instead of having to set up memory for a new instance each time, we only have to set up memory for that one instance, which is referenced throughout the application. However, Singletons are actually considered an anti-pattern, and can (or.. should) be avoided in JavaScript.

A Singleton instance should be able to get referenced throughout the entire app. Global variables essentially show the same behavior: since global variables are available on the global scope, we can access those variables throughout the application.

Having global variables is generally considered as a bad design decision. Global scope pollution can end up in accidentally overwriting the value of a global variable, which can lead to a lot of unexpected behavior.

## Proxy Pattern

```txt
Intercept and control interactions to target objects
```

With a Proxy object, we get more control over the interactions with certain objects. A proxy object can determine the behavior whenever we're interacting with the object, for example when we're getting a value, or setting a value.

Generally speaking, a proxy means a stand-in for someone else. Instead of speaking to that person directly, you'll speak to the proxy person who will represent the person you were trying to reach. The same happens in JavaScript: instead of interacting with the target object directly, we'll interact with the Proxy object.

Let's create a **person** object, that represents John Doe.

```js
const person = {
  name: "John Doe",
  age: 42,
  nationality: "American"
};
```

Instead of interacting with this object directly, we want to interact with a proxy object. In JavaScript, we can easily create a new proxy by creating a new instance of **Proxy**.

```js
const person = {
  name: "John Doe",
  age: 42,
  nationality: "American"
};

const personProxy = new Proxy(person, {});
```

The second argument of Proxy is an object that represents the **handler**. In the handler object, we can define specific behavior based on the type of interaction. Although there are many methods that you can add to the Proxy handler, the two most common ones are `get` and `set`:

- `get`: Gets invoked when trying to access a property
- `set`: Gets invoked when trying to modify a property

Let's add handlers to the personProxy Proxy. When trying to modify a property, thus invoking the set method on the Proxy, we want the proxy to log the previous value and the new value of the property. When trying to access a property, thus invoking the get method on the Proxy, we want the proxy to log a more readable sentence that contains the key and value of the property.

```js
const personProxy = new Proxy(person, {
  get: (obj, prop) => {
    console.log(`The value of ${prop} is ${obj[prop]}`);
  },
  set: (obj, prop, value) => {
    console.log(`Changed ${prop} from ${obj[prop]} to ${value}`);
    obj[prop] = value;
  }
});
```

The proxy made sure that we weren't modifying the person object with faulty values, which helps us keep our data pure!

## Provider Pattern

```txt
Make data available to multiple child components
```

In some cases, we want to make available data to many (if not all) components in an application. Although we can pass data to components using **props**, this can be difficult to do if almost all components in your application need access to the value of the props. We often end up with something called **prop drilling**, which is the case when we pass props far down the component tree. Refactoring the code that relies on the props becomes almost impossible, and knowing where certain data comes from is difficult.

Let's say that we have one **App** component that contains certain data. Far down the component tree, we have a **ListItem**, **Header** and **Text** component that all need this data. In order to get this data to these components, we'd have to pass it through multiple layers of components.

```js
function App() {
  const data = { ... }

  return (
    <div>
      <SideBar data={data} />
      <Content data={data} />
    </div>
  )
}

const SideBar = ({ data }) => <List data={data} />
const List = ({ data }) => <ListItem data={data} />
const ListItem = ({ data }) => <span>{data.listItem}</span>

const Content = ({ data }) => (
  <div>
    <Header data={data} />
    <Block data={data} />
  </div>
)
const Header = ({ data }) => <div>{data.title}</div>
const Block = ({ data }) => <Text data={data} />
const Text = ({ data }) => <h1>{data.text}</h1>
```

Passing props down this way can get quite messy. It would be optimal if we could skip all the layers of components that don't need to use this data. 

This is where the Provider Pattern can help us out! With the Provider Pattern, we can make data available to multiple components. Rather than passing that data down each layer through props, we can wrap all components in a **Provider**.

A **Provider** is a higher order component provided to us by the **Context** object. We can create a Context object, using the **createContext** method that React provides for us.

```js
const DataContext = React.createContext()

function App() {
  const data = { ... }

  return (
    <div>
      <DataContext.Provider value={data}>
        <SideBar />
        <Content />
      </DataContext.Provider>
    </div>
  )
}
```

Each component can get access to the data, by using the **useContext** hook.

```js
const DataContext = React.createContext();

function App() {
  const data = { ... }

  return (
    <div>
      <SideBar />
      <Content />
    </div>
  )
}

const SideBar = () => <List />
const List = () => <ListItem />
const Content = () => <div><Header /><Block /></div>


function ListItem() {
  const { data } = React.useContext(DataContext);
  return <span>{data.listItem}</span>;
}

function Text() {
  const { data } = React.useContext(DataContext);
  return <h1>{data.text}</h1>;
}

function Header() {
  const { data } = React.useContext(DataContext);
  return <div>{data.title}</div>;
}
```

We no longer have to worry about passing props down several levels through components that don't need the value of the props, which makes refactoring a lot easier.

### (Dis)advantages

The Provider pattern/Context API makes it possible to pass data to many components, without having to manually pass it through each component layer. It reduces the risk of accidentally introducing bugs when refactoring code. We no longer have to deal with prop-drilling, which could be seen as an anti-pattern.

In some cases, overusing the Provider pattern can result in performance issues. All components that consume the context re-render on each state change. To make sure that components aren't consuming providers that contain unnecessary values which may update, you can create several providers for each separate usecase.

## Observer Pattern

```txt
Use observables to notify subscribers when an event occurs
```

With the observer pattern, we can **subscribe** certain objects, the **observers**, to another object, called the observable. Whenever an event occurs, the observable notifies all its observers!

An observable object usually contains 3 important parts:

- `observers`: an array of observers that will get notified whenever a specific event occurs
- `subscribe()`: a method in order to add observers to the observers list
- `unsubscribe()`: a method in order to remove observers from the observers list
- `notify()`: a method to notify all observers whenever a specific event occurs

An easy way of creating one, is by using an [ES6 class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).

```js
class Observable {
  constructor() {
    this.observers = [];
  }

  subscribe(func) {
    this.observers.push(func);
  }

  unsubscribe(func) {
    this.observers = this.observers.filter(observer => observer !== func);
  }

  notify(data) {
    this.observers.forEach(observer => observer(data));
  }
}
```

Awesome! We can now add observers to the list of observers with the subscribe method, remove the observers with the unsubscribe method, and notify all subscribes with the notify method.

Let’s build something with this observable. We have a very basic app that only consists of two components: a **Button**, and a **Switch**.

```js
export default function App() {
  return (
    <div className="App">
      <Button>Click me!</Button>
      <FormControlLabel control={<Switch />} />
    </div>
  );
}
```

We want to keep track of the user interaction with the application. Whenever a user either clicks the button or toggles the switch, we want to log this event with the timestamp. Besides logging it, we also want to create a toast notification that shows up whenever an event occurs! Whenever the user invokes the handleClick or handleToggle function, the functions invoke the notify method on the observer. The **notify** method notifies all subscribers with the data that was passed by the **handleClick** or **handleToggle** function!

```js
import { ToastContainer, toast } from "react-toastify";

function logger(data) {
  console.log(`${Date.now()} ${data}`);
}

function toastify(data) {
  toast(data);
}

observable.subscribe(logger);
observable.subscribe(toastify);

export default function App() {
  function handleClick() {
    observable.notify("User clicked button!");
  }

  function handleToggle() {
    observable.notify("User toggled switch!");
  }

  return (
    <div className="App">
      <Button>Click me!</Button>
      <FormControlLabel control={<Switch />} />
      <ToastContainer />
    </div>
  );
}
```

Whenever a user interacts with either of the components, both the **logger** and the **toastify** functions will get notified with the data that we passed to the **notify** method!

Although we can use the observer pattern in many ways, it can be very useful when working with **asynchronous**, **event-based data**. Maybe you want certain components to get notified whenever certain data has finished downloading, or whenever users sent new messages to a message board and all other members should get notified.
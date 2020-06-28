# Context API Tutorial
A guide to help you get started with context API

### Prereqs : [React.js](https://reactjs.org/)

### Introduction
If you've used **React.js**, you've probably come across the problem of state management and how tricky it gets when there are multiple components manipulating data. How do you ensure every component has the same copy of the state? Using [Redux.js](https://redux.js.org/) is one of the most popular ways of managing the state of your application from a single place - the store. However, _not everyone who is starting out with React.js wants to download and setup yet another framework_ (although, frameworks are good!). Maybe I don't want to mapStateToProps and matchDispatchToProps in every component, ya know? <br />
You've also probably come across the issue of prop drilling - say you have to pass props from  ```Container -> Components ->  Single Component```. Let's face it, sooner or later it will get tedious to keep track of passing props down the line of hierarchy. <br />

React.js' solution to these problems is **the [Context API](https://reactjs.org/docs/context.html)**

I've used it in a **real world example, a todo app**. The code is [here](https://github.com/prerna-p/task-app) and the app is live [here](https://prerna-p.github.io/task-app/). It utilizes browser local storage.
### How to use it (a template)
1. Declare context(s). <br />
Just like you would declare components under ```src/components```, create ```src/contexts``` for your project. 
```javascript
import React, { createContext } from 'react';
const MyFirstContext = createContext();

export default MyFirstContext;
```

2. Create the 'Provider': <br />
If you're context is small, I recommend combining the Provider and Context in the same file, which is what I am going to do
```javascript
import React, { createContext } from 'react';
export const MyFirstContext = createContext();
const MyFirstContextProvider = props => {
const list = [ 1, 2, 3, 4, 5]
const addToList = value => {}
..
const deleteFromList = value => {}
..
return (
  <MyFirstContext.Provider
    value = {{  // value is the prop that is passed to consuming components
      list,     // the main data of my app, a list of numbers 
      addToList, // a function to add a number to the list
      deleteFromList, // a function to delete a number from the list
    }}>
      {props.children}
    </MyFirstContext.Provider>)
}
export default MyFirstContextProvider'
```
what is ```{props.children}``` doing here? It refers to all the components that will be wrapped by this provider

3. Wrap your App with the provider: <br />
```javascript
..
..
import MyFirstContextProvider from './contexts/MyFirstContextProvider'

const App = () => {
  return (
      <MyFirstContextProvider>
        <MyComponent1 />
        <MyComponent2 />
      </MyFirstContextProvider>
  );
};
export default App
```

4. Utilize the props in a child component
```javascript
import { MyFirstContext } from '../contexts/MyFirstContext'

const PrintNumbers = () => {
    const { list } = useContext(MyFirstContext); // destructuring the list from context
    return (
      <div>
        {list.length ?
            (<ul>
                { list.map(num => 
                    return <li> num </li>;
                )}
            </ul>) : <div>> no list yet</div>}
      </div>
    )
}
```

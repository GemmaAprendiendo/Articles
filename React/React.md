# React Notes 1

## Create and run
To create the default React App, go to your terminal in Visual Studio Code and run

`npx create-react-app TheNameYouWantForYourApp `

To run it, move to that folder and `npm start `

If you are cloning a repo, you can create some app first, then replace the files with the repo, since the repo is going to have "the code files" and not all the React files that are needed to run it.

By default, the app will start running in the webbrowser at localhost:3000 (Angular ones start at 4200)

#### Never run npm run eject!! 

Note: you could also do npm install -g create-react-app to install it and then jus type create-react-app TheNameYouWantForYourApp in the folder where you want the app.  The -g indicates to install it globally, so it can be accessed from any folder. 

## A basic React app

I find this site extremely helpful to try your code (simple) and see how it will work.  Going to https://stackblitz.com/edit/react-ajunq2 will also give you an idea of how a React app works. 

You will have an index.html file (look for it under the public folder), which will start the whole thing.  This file will have a div with an id where the React component (the main one) will be loaded.  You need an id so you can access the element later.

You can have whatever you want in there, but it needs this
 `<div id="root"></div>`

There should also be an index.js file.  This file will have this, which is what will load the component into the element. 

`
render(<App />, document.getElementById('root'));`

The App could be:
```
class App extends Component {
  constructor() {
    super();
    this.state = {
      name: 'React'
    };
  }

  render() {
    return (
      <div>
        <Hello name={this.state.name} />
        <p>
          Type something here to show it :)
        </p>
      </div>
    );
  }
}
```
Notice the render and the return.  That is where we will put the React or HTML elements we want to see on the page.  

In the code above, there is another React component being used, Hello, which we are setting to the state value (state.name) we set in the constructor.  This component could be (Hello.js file):

```
import React from 'react';

export default ({ name }) => <h1>Hello, look what was passed to me: {name}!</h1>;
```
A package.json file will also be created when you create the React app.

## React Components

React components are created with JSX, and the files should have that extension.  JSX looks like a mix of JavaScript and HTML.  (The Hello.js above doesn't have any HTML in it, so that's just js)

In a JSX file, we can be writing JavaScript code, with what looks like HTML tags, but they are not HTML exactly, so some things will be different and if you forget, you may run into problems. For example, class is for HTML, and the JSX equivalent would be className.  Also, tabindex in HTML will become tabIndex in JSX. 

To use any variables from the JS part of the file into the elements in the file, add {}, like in `<h1>{title}</h1> `

You can create elements by using React.createElement, but I think it’s easier to type the element itself the way you want it

You can create components as classes or functions.  Creating the component as a class will let you extend from the Component class in React and it will provide more features. 
If you need a constructor (which will run once) you will need to create a class. 
It is possible to use state with functions using hooks, but if you create a class (which will extend the React.Component class), you can use state for multiple variables easily.  More on state later. 

## Function Component

An example of a function component would be: 

```
import React from "react";

function Whatever () {
    return(
            <React.Fragment>
                ..some things in here
             </React.Fragment>
        )
};
        

export default Whatever;
```

Or you could also get something passed to it.

```
function HelloThere(props) { 
     return (<h1>Hello There {props.name} </h1>) 
} 
export default HelloThere; 
```
#### Note:If you need to return more than one element, you will have to wrap those elements into a higher element or it will not work. 

```
function HelloThere() {    
    return (<div> 
       	 	<h1>Hello There </h1> 
                  <h2>hey</h2> 
            </div>); 
   } 
   export default HelloThere; 
```
Add the **export default** at the end so when you need to import this component into another one React will know that HelloThere is the component that it needs to use, otherwise you will have to specify it. Notice there is no render on the function. 

The function is a regular JavaScript function and could be done with this syntax too: 

```
const HelloThere = (props) => ( 
           <h1>Hello There you </h1>) 
```

## Class Component

This class is so simple that a function should be used instead, but this is how it would be. 

```
import React, {Component} from 'react'; 
class HelloThere extends Component {    
    constructor(props){     
        super(props);  
        //whatever we need to do here to set state if we are using it 
    } 
    
    render() { 
        return( 
            <React.Fragment> 
                <h1>Hello There you </h1> 
            </React.Fragment> 
    ) 
 }; } 
export default HelloThere; 
```
`<React/Fragment> `is used as a parent div so we are returning only one element at the top level. 

## Importing the components 

How we import a component on another file will depend on whether the file containing the component we want has a default or not.  (export default).

Say we have a file that has two components, Component1, Component2. 
In that file we can export both components with: 

`export { Component1, Component2 } `

Then when we are going to use them in another file we would need to do: 

`import {Component1, Component2} from './ componentsAreInThisFile' `

If we try to import Component1 with this, it will fail because Component1 was not exported as default. 
`import Component1 from './ componentsAreInThisFile' `

We could export components from the file, plus the default one this way: 

```
export {Component2}; 
export default Component1; 
```

Then we could import them this way (Component1 being the default one): 
`import Component1, {Component2} from './ componentsAreInThisFile'; `

Note: When importing, you could name the component whatever you want, it does not have to match the name of the component in the file you are importing.   But maybe better to just keep them the same.

## State and Props (No Hooks) 

### What is State? 

You use the state to store values that correspond to elements on your component.  When the element must change you update the state and React will know to update that element (or elements) based on the change.  This is how you do not have to render all the elements after something in one of them changes. 
For example, if you need to display a header with Hello or Bye depending on something on your code, you can use state to store the greeting, and then you would use that on your element `(<h1>{this.state.greeting}</h1>)`.  Then when you changed the value of the greeting React would know that it needs to render the `<h1>` again. 
When you have different components that may need the same state, instead of having each component have its own state and your code trying to keep up with communication among the different components, bring the state to the common ancestor of those components. 


### State – set it and update it 

You will set the state in the constructor by assigning it a value, but if you want to change the value later on you must use setState and not change the value directly. 

```
constructor() {   	
    super(); 
    this.state = {	name: 'React', name2:'So Cool '   }}; 
 } 
 ```
To update the state later, you will use setState, which can take only the new values, or also a function to be called once the state is updated by React. 

```
this.setState( {name2: someVariableOrValue}, this.someFunctionThatWillBeCalle dWhenStateChanges); 
```

The reason we can pass that second argument for React to call a function once the update has happened, is that React may not update the state value right away.  We could call this.forceUpdate() after this.setState though to make React update at that time. 


Note: if you are doing something and it should work but you keep getting an error, check if you forgot to use “this”. 

You can pass the state to a component inside your component (child component) inside props, but this is like passing an argument by value.  You will not be able to update the state in the parent component from the child component. 

### What if the child component needs to update the state of the parent component? 

One thing we could do is pass a function in the parent to be called by the child when something happens, and then have the parent change the state in that function. 

You have a canvas.jsx component.  This component imports another component called TextFieldWithButton.  These are the things that TextFieldWithButton takes as props: 

```
<TextFieldWithButton textForInput = "Enter you text" textForButton="To canvas!" functionToCall= {this.updateCanvas}/> 
```
All the attributes above will be passed to the TextFieldWithButton component as props.  The functionToCall attribute will pass {this.updateCanvas} which is a function in the parent that will update the state. 

In the child component (TextFieldWithButton), the parent function will be called this way. 

```
this.props.functionToCall(“someValueYouWantToPassBack”); 
```
functionToCall was this.updateCanvas from the parent, so that is what will be called.  The function in the parent will take an argument. 

### Taking in the props 

In the receiving component (class or function), we can just have props or we can name each prop we want to take. Name each one and access by name only: 

```
const Hello = ( {name, name2}) => {       
    return <h1>Hello {name} and {name2} !</h1> 
}
```

### Props vs props.children 

Props are passed in like we saw in the above examples.  Props.children is passed automatically  to components (we don’t need to name them).  

```
render() {        return ( 
         <div> 
           <Hello name={this.state.name} name2={this.state.name2}> 
             <h1>hello header</h1>  
             <h2>hello header 2</h2>o 
           </Hello> 
    </div> 
    )}  ; 

```

Then in Hello we can access those through {props.children} 

### Rerender when state did not change 

Sometimes you may need for something to render again even though the value being displayed from the state has not changed.  In that case, add a new state property that (you may call it key) and use it in the component similar to this

`<Component key={this.state.selectedLetter}/> `

Then change the key state when you want the component to re-render. 


## Useful methods you get with your React class component 

### render 

All your class components will need this method.  This is what will return what the element will look like. 

```
render() {     return( 
        <React.Fragment> 
            <input type="search"></input> 
            <button onClick={this.sendTheText}>Button</button>         
        </React.Fragment>                       )}; 

```

### Constructor 

Used for initializations and setting state.  If you are using regular functions (as opposed to arrow functions) you can also do the bindings here. 
The constructor is called before the component is mounted.  Calling super(props) should be the first line in the constructor, since otherwise props will be undefined in the constructor, which could lead to problems. 
Remember this is the only place where you can set state directly (without using setState()) 

### componentDidMount() 

This is called right after the component has rendered.  Put code that needs to happen only after the component is available here.  You should add any subscriptions here. 

### componentDidUpdate() 

This is called after the component is updated. 

### componentWillUnmount() 

Called right before the component is unmounted and destroyed.  This is where you should perform any clean up you may have to do.  This includes cancelling subscriptions you may have added in componentDidMount. 

## Basic Examples 

For each one of this create the react app with npm create-react-app 

### Example – Very simple app with very simple component 

Index.html: 

```
After me, the div element that will display the React component 
<div id="root"></div> 
```
Index.js: 

```
import React, { Component } from 'react'; import { render } from 'react-dom'; import Hello from './Hello'; 
 
class App extends Component {   
    constructor() 
    {     
        super();     
    }  
  
    render() {     
        return ( 
            <div>       
                <Hello />         
            </div> 
        ); 
    } 
}

render(<App />, document.getElementById('root')); 
```

Hello.js 

```
import React from 'react'; 
export default () => <h1>Helloooo !</h1>; 
```

### Example – The component takes one argument 

Index.html: 

```
After me, the div element that will display the React component 
<div id="root"></div> 
```

Index.js: 

```
import React, { Component } from 'react'; 
import { render } from 'react-dom'; import Hello from './Hello'; 
 
class App extends Component {   
    constructor() {     
        super();     
    } 
render() {     return ( 
      <div>       
        <Hello name="Willie" />       
      </div> 
    ); 
  } } 
render(<App />, document.getElementById('root')); 

```
hello.js: 

```
import React from 'react'; 
export default ( {name}) => <h1>Helloooo {name}!</h1>; 
```

## Example – The component takes arguments from props 

Index.html: 

```
After me, the div element that will display the React component 
<div id="root"></div> 

```
Index.js: 

```
import React, { Component } from 'react'; 
import { render } from 'react-dom'; import Hello from './Hello'; 

class App extends Component {   
    constructor() 
    { 
        super();     
    }    
    
    render() { return ( 
      <div>       
        <Hello name="Willie" lastName="Wilson" />       
      </div> 
    ); 
  } 
}  
render(<App />, document.getElementById('root')); 

```

Hello.js 

```
import React from 'react'; 
 
function Hello(props){ 
  return <h1>Helloooo {props.name}, {props.lastName}!</h1>;   
} 
export default Hello 

```

### Example – Same as above, but with class component 

The only file that changes is the Hello component. 

```
import React , {Component} from 'react'; 
class Hello extends Component{   
    constructor(props){     
        super(props); 
        //nothing to do 
    }   
    render(){ 
        return <h1>Helloooo {this.props.name}, {this.props.lastName}!</h1>;   
    }     
} 
export default Hello 
```
### Example – Using state 

The example similar to the above ones, but the Hello component has changed. 

```
import React , {Component} from 'react'; 
class Hello extends Component
{   
    constructor(props)
    {     
        super(props); 
        this.state = {display:this.props.name}; 
    } 
    changeState = () => {     
        let date = new Date();     
        this.setState({display: this.props.name + "-" + date.getSeconds()});         
    }   
    
    render(){     
        return(  
            <React.Fragment> 
                <h1>Helloooo {this.state.display}!</h1> 
                <button onClick={this.changeState}>Change State</button> 
            </React.Fragment> 
        ) 
    }     
}

export default Hello 

```















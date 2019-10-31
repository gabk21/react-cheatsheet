# React Cheatsheet
Set of basic functionalities from React in one place. 

Cheatsheet was created by https://foreach.pl trainers for their students.

# Table of Contents

<!--ts-->
   * [Basic React commands](#Basic-React-commands)
   * [JSX](#JSX)
      * [Injection JS into HTML](#Injection-JS-into-HTML)
      * [Element declaration](#Element-declaration)
      * [Function injection](#Function-injection)
      * [Attributes](#Attributes)
      * [Fragments](#Fragments)
      * [References](#References)
   * [Components](#Components)
      * [Function component](#Function-component)
      * [Class component](#Class-component)
      * [Events](#Events)
      * [Conditional element](#Conditional-element)
      * [List of elements](#List-of-elements)
      * [Content projection](#Content-projection)
      * [Higher order component](#Higher-order-component)
   * [Routing](#Routing)
      * [Nested routing](#Nested-routing)
      * [Routing with parameters](#Routing-with-parameters)
      * [Authentication](#Authentication)
   * [State and life cycle](#State-and-life-cycle)
      * [State](#State)
      * [Life cycle](#Life-cycle)
   * [Forms](#Forms)
   * [Http requests](#Http-requests)
   * [Tests](#Tests)
      * [Interaction](#Interaction)
      * [Mocking http](#Mocking-http)
      * [Mocking components](#Mocking-components)
   * [Context](#Context)
   * [Hooks](#Hooks)
      * [Basic hooks](#Basic-hooks)
      * [Other hooks](#Other-hooks)
      * [Custom hook](#Custom-hook)
   * [TypeScript in React](#TypeScript-in-React)
      * [Add to project commands](#Add-to-project-commands)
      * [Add type for custom DOM element](#Add-type-for-custom-DOM-element)
   * [Interview questions](#Interview-questions)

<!--te-->

 Basic React commands
=================

| Command  | Notes | 
| ------------- | ------------- | 
| npx create-react-app {app name} | Create new React app |
| npm run build  | Build React app - create folder with files ready for deployment | 
| npm start  | Run project locally | 
| npm test  | run tests in React app |
| npm eject | remove the single build dependency from your project |

 

JSX
=================

JSX allows us to write HTML elements in JavaScript and place them in the DOM. Basically, it converts HTML tags into react elements.

## Injection JS into HTML

```jsx
 const value = 'Hello world'
 return ( <div>{value}</div>);
```
'Hello world' will be injected and displyed inside `<div>` element

## Element declaration

```jsx
 const value = 'Hello World'
 const element = <div className="App">{value}</div>
 return (element);
```
We can assign specific DOM fragment to particular variable (element above) and then (re)use it everywhere.

## Function injection

```jsx
 function addNumbers(x1, x2) {
   return x1 + x2;
 }
 const element = <div className="App">{addNumbers(2, 5)}</div>
```
Not only string values can be injected into DOM. Result of above case will be div with 7 inside as addNumbers was injected into element.

## Attributes
```jsx
  const avatarUrl = "img/picture.jpg"
  const element = <img src={user.avatarUrl}></img>;
```
JavaScript code can be also called for DOM element properites/events

## Fragments
JSX sytax must have one top parent HTML element. When we don't want pack our html into for example one div or other element then we can use `Fragment` which won't be rendered in our html

Example:
```jsx
<React.Fragment>
	<td>val1</td>
	<td>val2</td>
</React.Fragment>
```

or

```jsx
<>
  <td>val1</td>
  <td>val2</td>
</>

```

## References
We can refer to html element from JS

1. Declare reference
```js
this.element = React.createRef();
```

2. Assign reference to element
```jsx
<div ref={this.element}></div>
```

3. Use reference to interact with element
```js
this.element.current.focus();
```

Tip: References works for class component but not for function component

Components
=================

"React lets you define components as classes or functions. Components defined as classes currently provide more features"

## Function component

Sample component:
```js
function App() {
  return <div>Hello World</div>;
}
export default App;
```

Sample component with parameters:

```js
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}
```
```jsx
<Welcome name="Sara" />
```

## Class component

Sample component:
```js
class App extends React.Component  {
  render() {
    return <div>Hello World</div>;
  }
}
export default App;
```

Sample component with parameters:

```js
class Welcome extends React.Component  {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
export default App;
```
```jsx
<Welcome name="Sara" />
```

## Events

event in element:
```jsx
<a href="#" onClick={this.onClick}> Click me </a>
```

onClick function:
```jsx
onClick(e) {
    e.preventDefault();
    // some logic here
}

```

**Access to "this" inside event:**

1. bind this
```js
this.onClick = this.onClick.bind(this);
```

2. Arrow function 
```js
onClick = (e) => {
    e.preventDefault();
    console.log(this);
}
```

or

```jsx
 <a href="#" onClick={(e) => this.onClick(e)}> Click me</a>
```

**Child event**
We can call function in parent component from child component.
Child component (somewhere in code):
```js
this.props.onChange(val1, val2,...)
```

Parent component: 

```jsx
 <Child 
   onChange={hasChanged} />
```

```js
function hasChanged(val1, val2,...) {
 // some logic here
}
```

## Conditional element

Show element depending of implemented condition

**Option 1:**
```js
function SomeElement(props) {
   const show = props.isShowed;
   if(show) {
      return <div>Here is element</div>;
   }         
}
```
then: 
```jsx
<SomeElement  isShowed  = {true} />
```

**Option 2:**
```js
let element;      
if(this.state.isShow) {
    element  = <div> Here is element </div>;
} 
```

and then use element variable in jsx

**Option 3:**
```jsx
{this.state.isShow  ?  <div> Here is element </div> : 'No element'}
```

## List of elements

Recommended way to show list of same component is use "map" function. Example:
```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((numer, index) =>
  <li key={number.toString()}>
    {number}
  </li>
);

```
Then just use `listItems` element in your jsx DOM

## Content projection

Content projection is inject some html from parent component to child component

Example:

Parent component:
```jsx
<MyElement><h1>Hello world</h1></MyElement>
```

Child (MyElement) component:
```jsx
<div>
	{this.props.children}
</div>
```

`<h1>Hello world</h1>` will be injected to place where `{this.props.children}`, so MyElement will look like:

```jsx
<div>
	<h1>Hello world</h1>
</div>
```

## Higher order component
Create a parameterized component in order to reuse it in different ways.

Example:
```jsx
const myHoc = settings => WrappedComponent => {
    return class DecoratingComponent extends React.Component {
        render() {
            return (<div className={settings}><WrappedComponent {...this.props} /></div>)
        }
    }
}
```

Usage:
```jsx
const MyWrappedComponent = myHoc('test')(SomeComponent);
<MyWrappedComponent/>
```
`test` will be injected into className 


Routing
=================
Router will be described for react-router-dom library, which I can recommend. 

Install:
```
npm install react-router-dom 
```

Sample for area where components matching to current url will be rendered:

```jsx
<Router>
   <Switch>
     <Route path="/help"><Help /></Route>
     <Route path="/list"><List /></Route>
     <Route path="/"><Home /></Route>
   </Switch>
 </Router>

```

Link changing current url:

```jsx
<Link className="nav-link" to="/list">List</Link>
```

Tip: Route and Link need to be in same Router element

## Nested routing

Parent:
```jsx
<Route path="/some" component={SomeComponent}></Route>
```

SomeComponent: 
```jsx
function  SomeComponent({ match }) {
    return (
        <Router>
            <Switch>
                <Route path={`${match.path}/child`}><Child /></Route>
            </Switch>
        </Router>
    )
}
```

## Routing with parameters

```jsx
<Route path="/some/:id" component={SomeComponent}></Route>
```

and then in SomeComponent we have access to:

```jsx
{match.params.id}
```

## Authentication

Good idea is to create custom Route component that will show specific component only when our authentication logic is passed:

```jsx
function PrivateRoute({ component: Component, ...rest }) {
    return (
        <Route {...rest} render={(props) => (_isAuthentictedLogic_) ?
            (<Component {...props} />) :
            (<Redirect to={{ pathname: "/noAccess", state: { from: props.location } }} />
            )}
        />
    );
}
```

Then instead of Route use PrivateRoute

```jsx
<PrivateRoute path="/list" component={List}></PrivateRoute>
```


State and life cycle
=================

## State
The state decides about the update to a component’s object. When state changes, the component is re-rendered.

Declaring initial state: 
```js
constructor() {
    this.state = { userId: 2, userName: "Jannice"};
}
```

Using state:
```jsx
<div> this.state.date </div>
```

Updating state:
```js
this.setState({ userId: 3, userName: "Tom"});
```
After setState component will be re-rendered 

## Life cycle

| Life cycle  | Notes | 
| ------------- | ------------- | 
| componentDidMount | Called atthe beggining of component life - when component has been rendered |
| componentWillUnmount | This method is called before the unmounting of the component takes place.  |
| componentWillUpdate | Before re-rendering component (after change)  |
| componentDidUpdate | After re-rendering component (after change)  |
| componentDidCatch | called when error has been thrown |
| shouldComponentUpdate | Deterimines whether component should be updated after change or not  |


Example:
```jsx
componentDidMount() {
    console.log("componentDidMount")
    // some logic here
}

componentWillUnmount() {
    console.log("componentWillUnmount")
    // some logic here
}
```

Forms
=================

Sample form:

```js
onValueChange  = (event) => {
    this.setState({ value:  event.target.value  });
}
```
```js
onSubmit = (e) => {
	e.preventDefault();
	// Submit logic
}

```
```jsx
<form onSubmit={this.handleSubmit}>
    <input type="text"
        value={this.state.value}
        onChange={this.onValueChange}
    />
    
    <input type="submit" value="Send" />
</form>
```

Validation is described here: https://webfellas.tech/#/article/5

Http requests
=================

There are a lot of ways to communicate with our backend API. One of them is to use axior library.

Install:
`npm install axios`

Example with GET:
```js
axios.get('/user', {
params: {ID: 12345}
}).then(function (response) {
	//some logic here
})
.catch(function (error) {
	//some logic here
})
.finally(function () {
	//some logic here
}); 
```

Example with POST:
```js
axios.post('/user', {
    firstName: 'Fred',
    id: 2
}).then(function (response) {
	//some logic here
})
.catch(function (error) {
	//some logic here
});
```

Example with async call:
```js
const response = await axios.get('/user?ID=12345');
```

Tests
=================

Useful libs:

`npm install --save-dev jest`  to run tests
`npm install --save-dev @testing-library/react` Set of functions that may be useful for tests

Sample tests:

```jsx
let container = null;
beforeEach(() => {
  container = document.createElement("div");
  document.body.appendChild(container);
});
afterEach(() => {
  unmountComponentAtNode(container);
  container.remove();
  container = null;
});
it("renders menu with Help", () => {
  act(() => {
    render(<Router><SomeComponent /></Router>, container);
  });
  expect(container.textContent).toBe("Some text inside");
});

```

## Interaction

Click on element:
```js
import { fireEvent } from '@testing-library/react'
```
```js
fireEvent.click(container.querySelector('.some-class));
```

Change value in input:
```js
fireEvent.change(container.querySelector('.some-class)), {
    target: { value: "12345" }
});
```

Check if element contains class
```js
expect(someEl.classList.contains('disabled')).toBe(false);
```

## Mocking http

`npm install axios-mock-adapter --save-dev`

Example:
```js
var axios = require('axios');
var MockAdapter = require('axios-mock-adapter');
var mock = new MockAdapter(axios);

```
```js
mock.onGet('/users').reply(200, {
  users: [
    { id: 1, name: 'John Smith' }
  ]
});
```

With specific parameter
```js
mock.onGet('/users', { params: { searchText: 'John' } }).reply()
```

## Mocking components
Example:
```js
jest.mock("./SomeComponent", () => {
  return function DummyComponent(props) {
    return (
      <div data-testid="square">
        here is square
      </div>
    );
  };
});

```

Context
=================

Context is a "bag" for some data common for node of components.

Declare context:
```js
import React from 'react';
export const ColorContext = React.createContext('red');
```
'red' is default value

Create context and distribute it to child component:
```jsx
const { Provider } = ColorContext;
return (
    <Provider value={this.state.color}>
    	<SomeComponent />
    </Provider>
);
```

Use it in child component: 
```js
const { Consumer } = ColorContext;
return (
	<Consumer>
		{value => <div>{value}</div>}
	</Consumer>
)
```

Use it in child component outside render function: 
```js
static contextType = ColorContext;
let value = this.context.value
```

Hooks
=================
Hooks let us use in functions React features dedicated previosuly for only classes, like state.


## Basic hooks

**useState hook**

Can replace state managment
```js
const [count, setCount] = useState(0);
```
`count` can be used to read data
`0` is default value of count
`setCount(10)` is function to change count state value

**useEffect hook**

Can replace componentDidMount, componentDidUpdate, componentWillUnmount and other life cycle component events

Instead of using componentDidMount, componentDidUpdate we can use useEffect hook like this:
```js
useEffect(() => {
    document.title = `Clicked ${count}`;
});
```

In ordere to act as componentWillUnmount:
```js
useEffect(() => {
    return () => {/*unsubscribe*/};
});
```


**useContext hook**

Hooks for context implementation. See [Context](https://github.com/delprzemo/react-cheatsheet#context "context") 

Parent:
```jsx
const CounterContext = React.createContext(counter);

function App() {
  return (
    <CounterContext.Provider value={10}>
      <ChildComponent />
    </CounterContext.Provider>
  );
}
```

Child:
```jsx
const counter = useContext(CounterContext);
```

## Other hooks

| Hook  | Usage | Notes |
| ------------- | ------------- | ------------- | 
| useReducer | const [state, dispatch] = useReducer(reducer, initialArg, init); | Used for redux |
| useCallback | const memoizedCallback = useCallback(() => {doSomething(a, b);},[a, b],); | Returns a memoized callback |
| useMemo | const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]); | Returns a memoized value. |
| useRef | const refContainer = useRef(initialValue); | Hooks for element reference |
| useDebugValue | useDebugValue(value) | Display information in React DevTools |

## Custom hook
Custom hooks can be created.
Sample implementation:
```jsx
function useColor(initialColor) {
    const [color, changeColor] = useState(initialColor);
    function change(color) {
        changeColor(color);
    }
    useEffect(() => {
        console.log('Color changed');
    });
    return [color, change];
}

```


TypeScript in React
=================

## Add to project commands

`npx create-react-app my-app --typescript` - new app with TypeScript
`npm install --save typescript @types/node @types/react @types/react-dom @types/jest` - add TypeScript to existing project
`npx tsc --init` - init tsconfig

## Add type for custom DOM element

```ts
declare namespace JSX {
    interface IntrinsicElements {foo: any}
}
<foo />; // ok
<bar />; // error
```


Interview questions
=================

**Q: How can we declare the component??**

A: Use class extending React.Component or use function with React.FC type returning JSX. 

**Q: What is a difference between function component and class component?**

A: Components defined as classes currently provide more features. Anyway, hooks can change that.

**Q: In what type of component can we use hooks?**

A: Function component

**Q: How can we inject some html from parent component to child component?**

A: Using content projection - > this.props.children will keep parent html element. 
See [Content projection](https://github.com/delprzemo/react-cheatsheet#content-projection "content-projection") 

**Q: Describe useEffect hook**

A: UseEffect can replace componentDidMount, componentDidUpdate, componentWillUnmount and other life cycle component events.
See [Basic hooks](https://github.com/delprzemo/react-cheatsheet#basic-hooks "Basic-hooks") 

**Q: What is the difference between state and props??**

A: The state decides about the update to a component’s object. When state changes, the component is re-rendered.
Props are parameters passed from parent component to child component. 

**Q: What is a higher order component??**

A: Basically its's parametrized component. It allows to reuse particular component in many ways.
Sample implementation:
```jsx
const myHoc = settings => WrappedComponent => {
    return class DecoratingComponent extends React.Component {
        render() {
            return (<div className={settings}><WrappedComponent {...this.props} /></div>)
        }
    }
}
```

usage: 
```jsx
const MyWrappedComponent = myHoc('test')(SomeComponent);
<MyWrappedComponent/>
```

**Q: What is JSX?**

A: JSX allows us to write HTML elements in JavaScript and place them in the DOM. Basically, it converts HTML tags into react elements. See [JSX](https://github.com/delprzemo/react-cheatsheet#JSX "JSX") 

**Q: How can we create new react app?**

A: Using command `npx create-react-app {app name}`

**Q: What will be result of `npm eject`?**
A: Webpack for React application won't be handled automatically anymore. We will have access to Webpack configuration files in order to cusomize it to our needs. 



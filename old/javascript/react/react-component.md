# React component

App component. It's just a function (**function component**)

```
export default function App() {
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}
```

props - received fun parameters

jsx - returned HTML

## Html attributes in the component

```
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
      <h1>Hello {title}</h1>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div> 
  );
}
export default App;
```

In JSX HTML attributes are converted to camelCase presentation. We can use our own attributes. Examples:

* htmlFor - for
* className - class
* onClick - onclick

[all HTML attributes](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes)

## JS code in JSX

Everything in curly bracers can be used for JS expressions.

```
const anObject = {
    greeting: "Hello",
    name: "Mr. M"
}

const jsx = (
    <div className="App">
        {anObject.greeting} {anObject.name} {getRealName()}
    </div>
);

function getRealName() {
    return "Stranger"
}
```

## List of components

```
const data = [
    {
        title: 'React',
        url: 'https://reactjs.org/',
        author: 'Jordan Walke',
        num_comments: 3,
        points: 4,
        objectID: 0,
    }, {
        title: 'Redux',
        url: 'https://redux.js.org/',
        author: 'Dan Abramov, Andrew Clark',
        num_comments: 2,
        points: 5,
        objectID: 1,
    },
];

<div>
    {data.map(item => <div key={item.objectID}> {item.title} </div>)}
</div>
```

We avoid using the index of the item in the array to make sure the **key attribute is a stable identifier**. If the list changes its order, for example, React will not be able to identify the items properly.

## Functional component

```
// function declaration
function MyFuncComponent() {
    return <div>Function component</div>
}

// arrow function declaration
const MyArrowComponent = () => <div>Arrow component</div>
```

##

## Class component

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

* has own state
* can manage lifecicle

Typically, in React constructors are only used for two purposes:

* Initializing
* Binding

**For a visual reference, check out** [**this lifecycle diagram**](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)**.**

## Component event handling

[all browser events](https://developer.mozilla.org/en-US/docs/Web/Events)

To handle some event define the function and pass it to the specific attribute of an element.

Example: element input has onChange attribute which handles this browser event.

```
function MyFuncComponent() {
    const handleChange = event => console.log(event)

    return <div>
        <h1>My Hacker Stories</h1>
        <label htmlFor="search">Search: </label>
        <input id="search" type="text" onChange={handleChange} />
    </div>
}
```

## Component props

Passing variables to one component from another.

```
export default function App() {
    return (
        <div>
            <hr/>
            <MyFuncComponent one={data} two={{title:"one more"}}/>
        </div>
    )
}

function MyFuncComponent(props) {
    const {one, two} = props
    return (
        <div>
            <h1>My Hacker Stories</h1>
            {one.map(item => <div>{item.title}</div>)}
            <div>{two.title}</div>
        </div>
    )
}
```

There, we can access arguments through the first function signature’s argument, called **props**.

React Props are used to pass information down the component tree;

## Component state

React state is used to make applications interactive. We’ll be able to change the application’s appearance by interacting with it.

```
export default function App() {
    const [searchTerm, setSearchTerm] = React.useState('');

    const handleChange = event => {
        setSearchTerm(event.target.value);
    };

    return (
        <div>
            <h1>My Hacker Stories</h1>
            <label htmlFor="search">Search: </label>
            <input id="search" type="text" onChange={handleChange} />
            <p>
                Searching for <strong>{searchTerm}</strong>.
            </p>
            <hr />
        </div>
    );
}
```

[useState hook](https://reactjs.org/docs/hooks-state.html) **is a function** that returns an **array**, where the first element is the **current state**, and the second is **changing state function**.

After the new state is set in a component, the component renders again, meaning the component function runs again. The new state becomes the current state and can be displayed in the component’s JSX.

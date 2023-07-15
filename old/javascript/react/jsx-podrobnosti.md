# JSX подробности

Фундаментально, JSX является синтаксическим сахаром для функции

```
React.createElement(component, props, ...children)
```

JSX код:

```
<MyButton color="blue" shadowSize={2}>
    Click Me
</MyButton>
```

компилируется в:

```
React.createElement(
    MyButton,
    {color: 'blue', shadowSize: 2},
    'Click Me'
)
```

Также можно использовать самозакрывающую форму для тегов, у которых нет потомков. Например:

```
<div className="sidebar" />
```

компилируется в:

```
React.createElement(
    'div',
    {className: 'sidebar'},
    null
)
```

Протестировать, как различные конструкции JSX компилируются в JavaScript, можно в [онлайн компиляторе Babel](https://babeljs.io/repl/#?babili=false\&evaluate=true\&lineWrap=false\&presets=es2015%2Creact%2Cstage-0\&code=function%20hello\(\)%20%7B%0A%20%20return%20%3Cdiv%3EHello%20world!%3C%2Fdiv%3E%3B%0A%7D)

React и сам компонент должны быть в области видимости

```
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
     // return React.createElement(CustomButton, {color: 'red'}, null);
     return <CustomButton color="red" />;
}
```

Если вы не используете какой-либо упаковщик JavaScript и добавляете React непосредственно в тег \<script>, то React всегда будет находиться в глобальной области видимости.

### На компонент React можно ссылаться через точку в JSX:

```
import React from 'react';

const MyComponents = {
     DatePicker: function DatePicker(props) {
         return <div>Imagine a {props.color} datepicker here.</div>;
     }
}

function BlueDatePicker() {
     return <MyComponents.DatePicker color="blue" />;
}
```

### Выбор типа во время выполнения

Нельзя использовать общее выражение в типе элемента React. Это часто необходимо для отображения разных компонентов в зависимости от значения свойства в props:

```
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
     photo: PhotoStory,
     video: VideoStory
};

function Story(props) {
     // Неправильно! JSX тип не может быть выражением.
     return <components[props.storyType] story={props.story} />;
}
```

```
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
     photo: PhotoStory,
     video: VideoStory
};

function Story(props) {
     // Правильно! JSX тип может быть переменной, именуемой с Прописной буквы.
     const SpecificStory = components[props.storyType];
     return <SpecificStory story={props.story} />;
}
```

## Properties in JSX

For MyComponentcomponent value props.foo will be 10, because 1+2+3+4 = 10

```
<MyComponent foo={1 + 2 + 3 + 4} />
```

Constructions like if anf for cannot be used in JSX directly

```
function NumberDescriber(props) {
    let description;
    if (props.number % 2 == 0) {
        description = <strong>even</strong>;
    } else {
        description = <i>odd</i>;
    }
    return <div>{props.number} is an {description} number</div>;
}
```

###

It\`s the same

```
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

value will be automatically de-shielded

```
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

By default properties sets true

```
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

### Spreading attributes

... using as spread operator for pass all properties from object:

```
function App1() {
    return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
    const props = {firstName: 'Ben', lastName: 'Hector'};
    return <Greeting {...props} />;
}
```

## props.children в JSX

props.children for MyComponentis set to "Hello world!"

```
<MyComponent>Hello world!</MyComponent>
```

### JSX элемент

Other JSX elements can act as children of a JSX element.

```
<MyContainer>
    <MyFirstComponent />
    <MySecondComponent />
</MyContainer>
```

### JavaScript expressions

You can use any JavaScript expression as a child

```
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

```
function Item(props) {
    return <li>{props.message}</li>;
}

function TodoList() {
    const todos = ['finish doc', 'submit pr', 'nag dan to review'];
    return (
        <ul>
            {todos.map((message) => <Item key={message} message={message} />)}
        </ul>
    );
}
```

### function JavaScript

props.children can be callback function

```
function ListOfTenThings() {
    return (
        <Repeat numTimes={10}>
          {(index) => <div key={index}>This is item {index} in the list</div>}
        </Repeat>
    );
}
```

```
function Repeat(props) {
    let items = [];
    for (let i = 0; i < props.numTimes; i++) {
        items.push(props.children(i));
    }
    return <div>{items}</div>;
}
```

### boolean, Null и Undefined is ignoring

false, null, undefined и true are not displaying while rendering component:

```
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{true}</div>
```

It can be usefull for conditional rendering

```
<div>
    {showHeader && <Header />}
    <Content />
</div>
```

```
<div>
    {props.messages.length > 0 &&
        <MessageList messages={props.messages} />
    }
</div>
```

Первоисточник: [React — Advanced Guides — JSX In Depth](https://facebook.github.io/react/docs/jsx-in-depth.html)

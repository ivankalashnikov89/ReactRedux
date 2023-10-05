##### MY REACT AND REDUC TOOLKIT JOURNEY

##### First Component

```js
    function Greeting() {
        return <h2>My first Component</h2>
    }

//Arrow function also works
    const Greeting = () => {
        return <h2>My first Component</h2>
    }

```
-starts with a capital letter
-must return JSX(html)
-always close tag <Greeting/>

##### Typical Component

```js
// imports or logic
    const Greeting =() => {
        return <h2>Component</h2>
    }
    export default Greeting;
```

##### Root Component(only one)

index.js

```js
import React from "react";
import ReactDOM from "react-dom";

const Greeting =() => {
        return <h2>Component</h2>
    }
    
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(<Greeting/>)
```

##### First Component in detail

-capital letter
-must return something
-JSX syntax(return html)
    -to make our live easier
    -calling function under the hood

index.js

```js
    const Greeting = () => {
        return React.createElement('h2',{},'hello')
    }
```

##### JSX Rules

-return single element
    -semantic section/article
    -Fragment - let's us group elements without adding extra nodes

```js
    return <React.Fragment>....return something...</React.Fragment>

    // shorthand

    return <>....return something...</>
```

 -className instead of class

 ```js
    return <div className="class">hi</div>
 ```

 - close every element

 ```js
    return <img />
    return <input /> 
 ```


 ##### Nest Components

 ```js
    function Greeting() {
        return (
            <div>
                <Person />
                <Message />
            </div>
        );
    }

const Person = () => <h2>John</h2>;
const Message = () => {
    return <p>Message</p>
}

 ```

##### Props - Initial Setup

 - refactor / clean up

 ```js
 const author = "JRRTolkien";
 const title = "The Lord of the Rings";

function Book() {
    return (
        <article className = "class_name">
            <h2>{title}</h2>
            <h4>{author}</h4>
        </article>
    )
}
 ```

##### Props - Somewhat Dynamic Setup

- setup an object
- refactor vars to properties
- copy/paste and rename
- get values for second book
- setup props

```js 
    const firstBook = {
        author:"JRR Tolkien",
        title:"Silmarillion"
    };
    const secondBook = {
        author:"J Rowling",
        title:"Harry Potter"
    }

    const BookList = () => {
        return (
            <section className = "className">
                <Book
                    author = {firstBook.author}
                    title = {firstBook.title}
                />
                <Book
                    author = {srcondBook.author}
                    title = {secondBook.title}
                />
            </section>
        );
    };

    const Book = (props) => {
        return (
            <article className = "className">
                <h2>{props.title}</h2>
                <h3>{props.author}</h3>
            </article>
        );
    };

```


##### Children Prop

- everything we render between compnent tags
- during the course we will mostly use it Context API
- special prop, has to be 'children'
- can place anywhere in JSX








```js

    const App = () => {
        return "Hello React";
    }

```
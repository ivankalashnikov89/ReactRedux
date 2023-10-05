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



##### Mapping

- map - creates a new array from calling a function for every array element

```js
    const names = ["Ivan", "Alex", "Denis"]
    const newNames = names.map((name => {
        return <h1>{name}</h1>
    }));

    function BookList() {
        return <section>{newNames}</section>
    }
```

##### Pass the entire object

- render component
- pass entire object
- destructuring(object)

```js
    function BookList() {
        return (
            <section className = "className">
                {books.map((book) => {
                    const {title, author} = book;
                    return <Book book = {book}>
                })}
            </section>
        )
    }
```


##### EVENT - FUNDAMENTALS #####

- Vanilla JS

```js
    const btn = document.querySelector("#btn");

    btn.addEventListener('click', function(e) {
        //do something
    })
```

- similar approach
- element, event, function
- again camelCase

```js
    const handleClick = () => {
        alert("button clicked!")
    };
    return  (
        <section>
            <button onClick = {handleClick}>Click</button>
        </section>
    )
```

- [React Events] (https://reactjs.org/docs/events.html)
- no need to memorize 
- most common 
    - onClick(click events)
    - onSubmit(submit form)
    - onChange(input change)

```js
    function BookList() {
        return (
            <section>
                <EventExamples />
            </section>
        )

        const EventExamples = () => {
            const handleFormInput = () => {
                console.log("handle form input")
            };
            const handleButtonClick = () => {
                alert("handle button click");
            };

            return (
                <section>
                    <form>
                        <input 
                            type = "text"
                            name = "example"
                            onChange = {handleFormInput}
                        />
                    </form>
                    <button onClick = {handleButtonClick}>Click</button>
                </section>
            );
        };
    };
```
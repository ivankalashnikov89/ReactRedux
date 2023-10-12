##### MY REACT AND REDUC TOOLKIT JOURNEY

##### FIRST COMPONENT #####

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

##### TYPICAL COMPONENT #####

```js
// imports or logic
    const Greeting =() => {
        return <h2>Component</h2>
    }
    export default Greeting;
```

##### ROOT COMPONENT (ONLY ONE) #####

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

##### FIRST COMPONENT IN DETAIL #####

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

##### JSX RULES #####

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


 ##### NEST COMPONENTS #####
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

##### PROPS - INITIAL SETUP #####

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

##### PROPS - SOMEWHAT DYNAMIC SETUP #####

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


##### CHILDREN PROP #####

- everything we render between compnent tags
- during the course we will mostly use it Context API
- special prop, has to be 'children'
- can place anywhere in JSX



##### MAPPING #####

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

##### PASS THE ENTIRE OBJECT #####

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


##### EVENT OBJECT AND FORM SUBMISSION #####

```js
    const EventExamples = () => {
        const handleFormInput = (e) => {
            //e.target - element
            console.log(`Input Name : ${e.target.name}`);
            console.log(`Input Value : ${e.target.value}`);
        
        };
        const handleButtonClick = () => {
            alert("clicked");
        };
        const handleFormSubmission = (e) => {
            console.log("submitted");
        };
        return (
            <section>
                {/*add onSubmit event Handler*/}
                <form onSubmit = {handleFormSubmission}>
                    <input 
                        type = "text"
                        name = "example"
                        onChange = {handleFormInput}
                    />
                 <button type = "submit">Submit Form</button>   
                </form>
                <button onClick = {nahdleButtonClick}>Click</button>
            </section>
        );
    };
```

##### MIND GRENADE #####

- alternative approach
- pass anonymous function (in this case arrow function)
- one liner-less code

```js
    const EventExample = () => {
        return (
            <section>
                <button onClick = {() => console.log("hello")}>Click</button>
            </section>
        );
    };
```

##### MIND GRENADE #2 #####

- remove EventExamples
- components are Independent by default

```js
    function BookList() {
        return (
            <section>
                {books.map((book) => {
                    return <Book {...book} key = {book.id}>
                })}
            </section>
        );
    }

    const Book = (props) => {
        const {img, title, author} = props;
        const displayTitle = () => {
            console.log(title);
        };
    

    return (
        <article>
            <h2>{title}</h2>
            <button onClick={displayTitle}>display title</button>
            <h4>{author}</h4>
        </artice>
        );
    };
```


##### PROP DRILLING #####

- react data flow - can only pass props dows
- alternatives Context API, redux, other state libraries

```js
    function BookList() {
        const someValue = "shakeAndBake";
        const displayValue = () => {
            console.log(someValue);
        };
        return (
            <section>
                {books.map((book) => {
                    return <Book {...book} key ={book.id} displayValue = {displayValue}>
                })}
            </section>
        );
    }

    const Book = (props) => {
        const {title, author, displayValue} = props;

        return (
            <article>
                <h2>{title}</h2>
                <button onClick={displayValue}>Click</button>
                <h4>{author}</h4>
            </article>
        );
    };
```

##### MORE COMPLEX EXAMPLE #####

- Initial setup
- Create getBook function in bookList
- accepts id as an argument and finds the book
- pass function down to Book Component and invoke on the button click
- in the Book Component destructure id and function 
- invoke the function when user clicks the button, pass the id
- the goal: you should see the same book in the console

```js
    const BookList = () => {
        const getBook = (id) => {
            const book = books.find((book) => book.id === id);
            console.log(book)
        };

        return (
            <section>
                {books.map((book) => {
                    return <Book getBook = {getBook} />
                })} 
            </section>
        );
    };

    const Book = (props) => {
        const {author, title, getBook} = props;
        return (
            ....
            <button onClick = {getBook(id)}>Click Me</button>
            ....
        );
    };
```

##### LOCAL IMAGES (SRC FOLDER) #####

- better performance because optimized 
- add one more book to array
- downlaod all three images (rename)
- setup images folder in the src
- import all three images in the books.js
- set image property equal to import

```js
    import img1 from './images/book1.jpg';
    export const books = {
        author:'JRRTolkien',
        title:'The Lord of the Rings',
        img:img1
    }
```


###### ##### ##### ADVANCED REACT ##### ##### #####

##### The Need for State #####

- create count variable
- display value in the JSX
- add button and increase the value

```js
    const ErrorExample = () => {
        let count = 0;

        const handleClick = () => {
            count = count + 1;
        }
    }
```

##### useState Basics #####

- useState Hook
- returns an array with two elements: current state value, and a function that we can use to update the state
- accepts default value as an argument 
- state update triggers re-render

```js
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setCount(count + 1)
    };

    .....

    <button onClick = {handleClick}>Click me</button>
```

##### General Rules of Hooks #####

- starts with 'use' 
- component must be uppercase
- invoke inside function/component body
- don't call hooks conditionally
- set functions don't update state immediately 

##### useState with Array #####

##### useState with Object #####

```js
    const UseStateObject = () => {
        const [name, setName] = useState("Ivan");
        const [age, setAge] = useState(33);
        const [hobby, setHobby] = useState("Hockey")
    }

    const displayAlex = () => {
        setName("Alex");
        setAge(37);
        setHobby("Simpsons"   
    }



    const UseStateObject2 = () => {
        const [person, setPerson] = useState({
            name:"Ivan",
            age:33,
            hobby:"hockey"
        });

        const displayPerson = () => {
            setPerson({name:"ALex", age:36, hobby:"Simpsons"});
            //to change one parameter
            setPerson({...person, name:"Alex"});
        }
    }
```

##### useEffect Basics #####

useEffect is a hook in React that allows you to preform side 
effects in function components. There is no need for urban dictionary - 
basically any work outside of the component.

-useEffect hook
- accepts two arguments (second optional)
- first argument - callback function
- second argument - dependency array
- by default runs on each render 
- callback cannot return promise (so cannot make it async)
- it dependency array empty [] runs only on initial render

```js
    import {useState, useEffect} from "react";

    const url = 'http://api.github.com/userss';

    const FetchData = () => {
        const [users, setUsers] = useState([]);

        useEffect(() => {
            const fetchData = async () => {
                try {
                    const response = await fetch(url);
                    const users = await response.json();
                } catch(error) {
                    console.log(error)
                }
            };
            fetchData();
        }, []);
    };
```









##### REDUX TOOLKIT #####

##### DOCS #####

[ReduxToolkitDocs] (https://redux-toolkit.js)

##### Isntall Template #####

```sh
    npx create-react-app my-app --template redux
```

- @latest

```sh
    npx create-react-app@latest my-app --template redux
```

##### Existing App #####

```sh
    npm install @reduxjs/toolkit react-redux
```

##### @reduxjs/toolkit #####

consists of few libraries

    - redux (core library, state management)
    - immer (allows to mutate state)
    - redux-thunk(handles async actions)
    - reselect (simplifies reducer functions)

##### Extras #####

- redux devtools
- combine reducers

##### React-redux #####

connects our app to redux

##### Setup Store #####

- create store.js

```js
    import {configureStore} from "reduxjs/toolkit";

    export const store = configureStore({
        reducer:{},
    });
```

##### Setup Provider #####

- index.js

```js
    import React from "react";
    import ReactDOM from "react-dom";
    import App from './App';
    //import store and provider
    import { store } from './store';
    import { Provider } from 'react-redux';

    ReactDOM.render(
        <React.StrictMode>
            <Provider store = {store}>
                <App />
            </Provider>
        </React.StrictMode>,
        document.getElementById('root')
    );
```

##### Setup Cart Slice #####

- cartSlice.js

```js
    import { createSlice, createAsyncThunk } from 'reducjs/toolkit';

    const initialState = {
        cartItems:[],
        //!!!!!///
        amount:0,
        //!!!!!//
        total:0,
        isLoading:true
    };

    const cartSlice = createSlice({
        name:"cart",
        initialState
    });

    export default cartSlice.reducer;

```

-store.js

```js
    import { configureStore } from "reduxjs/toolkit";
    import cartReducer from ".../cartSlice";

    export const store = configureStore({
        reducer: {
            cart: cartReducer,
        },
    });
```

-Navbar.js

```js
    import { useSelector } from "react-redux";

    const Navbar = () => {
        //Accessimng data from cartSlice!!!!
        const {amount} = useSelector((store) => store.cart);
        return (
            .....
            //Show data!!!!
            <p> {amount}</p>
            .....
        );
    };
    export default Navbar;
```

##### First Reducer #####

- cartSlice.js
- Immer Library

```js
    const cartSlice = createSlice({
        name:"cart",
        initialState,
        reducers: {
            clearCart: (state) => {
                state.cartItems = [];
            };
        },
    });

    export const { clearCart } = cartSlice.actions;
```

- create action 

```js
    const ACTION_TYPE = 'ACTION_TYPE';

    const actionCreator = (payload) => {
        return { type: ACTION_TYPE, payload:payload};
    };
```

- CartContainer.js
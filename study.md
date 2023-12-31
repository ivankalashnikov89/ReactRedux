##### MY REACT AND REDUC TOOLKIT JOURNEY

##### HOW DOES REDUX WORK #####

    Redux enables you to maintain a single centrelized 
store that manages the state ot your entire app.
All components in your app can access this store and 
update or retrieve data from it as needed.

    The key components that enable this centrelized approach 
to state management are:
- Store
- Actions
- Dispatch
- Reducers

##### THE STORE #####

    The redux store is like a giant container that holds all
the data for your app.

    Think of the store as a box with different compartments 
for different data types. You can store any data you want
in these compartments, and it can hold various kinds of 
data, such as strings, numbers, arrays, objects, and even 
functions.

    Also, the store is the single source of truth for your 
app's state. This means that any component in your app can
access it to retrieve and update data.

##### ACTIONS #####

An action is an object that describes what changes need
to made to the state of your app. It sends data from your 
app to the Redux store and serves as the only way to
update the store.

And action must habe a 'type' property describing the 
action being performed. This 'type' property is 
typically defined as a string constant to ensure 
consistency and avoid typos.

In addition to the 'type' property, an action can have
a 'payload' property. The 'payload' property represents
the data that provides additional information about the
action being performed.For example, if an action type 
is ADD_TASK, the payload might be an object containing
a new task item's "id", "text" and "compeleted status".

EXAMPLE

```JS
    {
        type: 'ADD_TASK',
        payload: {
            id:1,
            text:"text",
            completed:false
        }
    }
```
Note that to create actions, we use action creators. 
Action creators are functions that create and return action 
objects.

EXAMPLE

```js
    function addTask(taksText) {
        return {
            type: 'ADD_TASK',
            payload: {
                id:1,
                text:"text",
                completed:false
            }
        }
    }
```

##### DISPATCH ##### 

In Redux, dispatch is a function provided by the store 
that allows you to send an action to update the state of
your app. When you call 'dispatch', the store runs as
action through all of the available reducers, which in 
turn update the state accordingly.

You can think of 'dispatch' as a mail carrier who 
delivers mail to different departments in a large company.
Just like how the mail carrier delivers mail to different
departments, 'dispatch' delivers actions to various reducers
in your Redux store. Each reducer is like a department in
the company that processes the mail and updates its own 
part of the company's data.

##### REDUCERS #####

In Redux, a reducer is a function that takes in the current
state of an app and an action as arguments, and returns a 
new state based on the action.

EXAMPLE

```js
    const initialState = {
        count: 0;
    };

    function counterReducer(state = initialState, action) {
        switch(action.type) {
            case 'INCREMENT':
                return {...state, count:state.count + 1};
            case 'DECREMENT':
                return {...state, count:state.count - 1};
            default:
                return state;
        };
    };
```
In the above code, we have a simple reducer called 
'counterReducer' that manages the state of a count 
variable. It takes in two arguments : state and action.
The state argument represents the current state of your 
app, while the action argument represents the action 
dispatched to modify the state.

##### REAL APP IMPLEMENTATION #####

##### HOW TO SET UP THE PROJECT #####

```sh
    npm create vite@latest project_name -- --template react
    cd project_name
    npm install
```

##### HOW TO INSTALL REDUX #####

    Redux requires a few dependencies fot its operations,namely:
        REDUX. The core library enables the redux architecture.
        REACT REDUX. Simplifies connecting your React 
            components to the Redux store.
        REDUX THUNK. Allows you to write async logic in 
            your Redux actions.
        REDUX DEVTOOLS EXTENSION. Connects your Redux app to
            Redux DevTools

```sh
    npm install \
    redux\
    react-redux\
    redux-thunk\
    redux-devtools-extension
```

##### HOW TO SET UP REDUCERS ##### 

    in the 'src' directory, create a new folder called
'reducers', and inside that folder, create two new files:
index.js and taskReducer.js

    The index.js file represents the root reducer, which
combines all the individual reducers in the app. In contrast, 
the taskReducer.js file is one of the individual reducers
that will be combined in the root reducer.

- index.js 

```js
    import taskReducer from './taskReducer'
    import { combineReducers } from 'redux'

    const rootReducer = combineReducers({
        tasks:taskReducer,
    });

    export default rootReducer;
```

    In the above index.js file, we use the combineReducers 
function to combine all the individual reducers into a single
root reducer. In this case, we only have one reducer
(taskReducer), so we pass it in as an argument to 
combineReducers.

    The resulting combined reducer is then exported so that 
other files in the app can import and use it to create the
store:

-taskReducer.js

```js
    const inititalState = {
        tasks: []
    };

    const taskReducer = (state = initialState, action) => {
        switch(action.type) {
            case "ADD_TASK":
                return {
                    ...state,
                    tasks: [...state.tasks, action.payload]
                };
            case "DELETE_TASK":
                return {
                    ...state,
                    tasks: state.tasks.filter(
                        task => task.id !== action.payload
                    )
                };
            default:
                return state;
        }
    }

    export default rootReducer;
```

    Inside the above taskReducer.js file, we define a reducer
function that takes two arguments: state and action. The state
argument represents the current state of the app, while the 
action argument represents the action being dispatched to
update the state.

    The 'switch' statement insife the reducer handles different
cases based on the "type" of the action. For example, if the
action type is ADD_TASK, the reducer returns a new state object
with a new task added to the tasks array. And if the action 
type is DELETE_TASK, the reducer returns a new state object with
the current tasks filtered to remove the taks with the specified "id".

##### HOW TO CREATE THE REDUX STORE #####

-store.js

```js
    import {createStore, applyMiddleware } from 'redux'
    import thunk from 'redux-thunk'
    import { composeWithDevTools } from 'redux-devtools-extension';

    import taskReducer from './taskReducer'

    const store = createStore(
        taskReducer,
        composeWithDevTools(applyMiddleware(thunk))
    );

    export default store;
```
    The code above sets up a Redux store by creating a new 
instance of the store using the 'createStore' function. 
Then, the rootReducer - which combines all the app's 
reducers into a single reducer - is passed as an argument 
to 'createStore'.

    The 'redux-thunk' library allows you to write async actions,
while the 'redux-devtools-extension' library enables you to use
the Resux DevTools browser extension to debug and inspect the state
and actions in the store.

    Finally, we export the store so we can use it in our app.
We use the 'composeWithDevTools' function to enhance the 
store with the ability to use the Redux DevTools extension, and
the 'applyMiddleware' function to apply the thunk middleware to 
the store.

##### HOW TO CONNECT THE REDUX STORE TO THE APP #####

    To connect the Redux store to the ToDo app, we need to use
the 'Provider' component from the 'react-redux' library.

    First, we import the 'Provider' function and the Redux store
we created into our 'main.jsx'. Then, we wrap our 'App' component
with the 'Provider' function and pass the 'store' as a prop. This
makes the Redux store available to all the components inside
the 'App'.

```js
    import React from 'react'
    import ReactDOM from 'react-dom/client'
    import App from './App'
    import './index.css'

    import { Provider } from 'react-redux'
    import store from './store'

    ReactDOM.createRoot(document.getElementById('root')).
        render(
            <React.StrictMode>
                <Provider store = {store}>
                    <App />
                </Provider>
            </React.StrictMode>
        )
```

##### HOW TO SET UP REDUX ACTIONS #####

    Actions represent something that happened in the app.
    To create the actions, create a new folder called 'actions'
in the 'src' directory and then create a new file called
'index.js'. This file will contain all of the action creators
for our app.

```js
    export default addToDo = (text) => {
        return {
            type:'ADD_TASK,
            payload: {
                id:new Date().getTime(),
                text:text
            },
        };
    };

    export const deleteToDd = (id) => {
        return {
            type:'DELETE_TASK',
            payload:id
        };
    };
```

    The above code exports two action creators:addTodDo and
deleteToDo. These functions return an object with a type 
property that describes the action that has occured.

    In the case of 'addToDo', the 'type' property is set to
'ADD_TASK', indicating that a new task has been added. The 
'payload' property contains an object with the new task's 'id'
and 'text' values. The 'id' is generated using the
'newDate().getTime() method creates a unique identifier 
based on the current timestamp.

    In the case of 'deleteToDo', the 'type' property is set to
'DELETE_TASK', indicating that a taks has been deleted. The 
'payload' property contains the 'id' of the task to be deleted.

    These action creators can be dispatched to the Redux store
using the 'dispatch()' method, which will trigger the 
corresponding reducer function to update the app state 
accordingly.

##### HOW TO DISPATCH ACTIONS #####

    Let's create a new folder named 'components' inside the 'src'
directory. Inside this folder, we will create two new files:
'Task.jsx' and 'TaskList.jsx'.

    The 'Task.jsx' component will be responsible for adding tasks. 
But before we proceed, we need to import the following into 
the fiie;
    -addToDo action: To add new tasks to the state;
    -useDispatch hook: To dispatch the 'addToDo' action;
    -useRef: Allows us to obtain a reference to HTML elements.

```js
    import { useRef } from 'react';
    import { useDispatch } from 'react-redux';
    import { addToDo } from '../actions';
```

    Once we have imported these necessary components, we can
proceed to write code for 'Task.jsx'.

```js
    const Task =() => {
        const dispatch = useDispatch();
        const inputRef = useRef(null);

        function addNewTask() {
            const task = inputRef.current.value.trim();
            if(task !== "") {
                dispatch(addToDo(task));
                input.current.value = "";
            }
        }

        retiurn (
            <div>
                <div>
                    <input 
                        type = "text"
                        placeholder = "text"
                        ref = {inputRef}
                    />
                    <button onClick = {addNewTask}>
                        New task
                    </button>
                </div>
            </div>
        );
    };

    export default Task;
```

    In the code above, we created a component consisting of an
input field and a button. When a user clicks on the 'Add task'
button, the 'addNewTask' function is executed. This function 
uses the 'useRef' hook to obtain the input field's value, 
removes any leading or trailing whitespaces, and then dispatched
the 'addToDo' action with the new taks as the payload.

    Now, let's move on to the 'TaskList.jsx' component, 
responsible for rendering the list ot tasks and handling taks
deletions. To achieve this, we need to import the following:
    -The 'useSelector hook' provides access to the state from 
the Redux store;
    -The 'deleteToDo action, is responsible for removing a task
from the list of tasks in the Redux store;

```js
    import { useSelector, useDispatch } from 'react-redux'
    import { deleteToDO } from './dispatch'
```

    We will now write code for 'TaskList.jsx' that maps over
the tasks array and renders each taks:

```js
    import React from 'react'
    import { useSelector, useDispatch } from 'react-redux'
    import { deleteToDO } from './dispatch'

    const TaskList = () => {
        const tasks = useSelector((state) => state.tasks);
        const dispatch = useDispatch();

        const handleDelete = (id) => {
            dispatch(deleteToDo(id));
        };

        return (
            <div>
                <div>
                    <h3>Your tasks:</h3>
                    <ul>
                        {tasks.map((task) => {
                            <li key = {task.id}>
                                {task.text}
                                <button
                                    onClick = {()=>handleDelete(task.id)}
                                >
                                    Delete
                                </button>
                            </li>
                        })}
                    </ul>
                </div>
            </div>
        )
    }

    export default TaskList;
```

    Here, the component loops over each task in the tasks array and
displays text and a delete button. When the user clicks the delete
button, the 'handleDelete' function is called, dispatching the 
'deleteToDo' action with the task's 'id' as the payload.

    Finally, import the component into your 'App.jsx' file and 
render them.

-App.jsx

```js
    import Task from './Task'
    import TaskList from './TaskList'

    function App() {
        return (
            <div>
                <Task />
                <TaskList />
            </div>
        );
    }

    export default App;
```

##### STYLING #####



##### HOW TO USE REDUX TOOLKIT #####

# ADVANTAGES OF REDUX TOOLKIT #
    Redux Toolkit provides several advantages over traditional Redux
        - It is easier to set up and requires fewer dependencies;
        - Reduces boilerplate code by allowing the creation of a single 
          file known as 'slice' that combines actions and reducers.
        - Provides sensible defaults for commonly used features, such
          as Redux Thunk and Redux DevTools. This means that you don't 
          have to spend time to configuring these features yourself,
          as they are already built into Redux Toolkit.
        - It uses the immer library under the hood, which enables 
          direct state mutation and eliminates the need for manually
          copying the state '{...state}' with every reducer.

##### HOW TO SET UP REDUX TOOLKIT ##### 

    To use Redux Toolkit in your React App, you need to install
two dependencies:@reduxjs/toolkit and react-redux

    The '@reduxjs/toolkit' package provides the necessary tools to 
simplify Redux development, while 'react-redux' is needed to 
connect your Redux store to your React components.

```sh
    npm install @reduxjs/toolkit react-redux
```

##### HOW TO CREATE A SLICE #####

    Once you have installed the needed dependencies, create a new
'slice' using the 'createSlice' function. A slice is a portion of 
the Redux store that is responsible for managing a specific piece 
of state.

    Think of the Redux store as a cake, where each slice represents
a specific piece of data in the store. By creating a slice, you
can define the behaviour of the state in response to particular actions 
using reducer function.

    To create a slice to manage our ToDo app, crete a new file 
named 'src/features/todo/todoSlice.js' and add the following code.

```js
    import { createSlice } from '@reduxjs/toolkit'

    const initialState = {
        tasks: [],
    };

    const todoSlice = createSlice({
        name:'todo',
        initialState,
        reducers: {
            addTodo:(state,action) => {
                state.tasks.push({
                    id:Date.now(), text: action.payload
                })
            },
            deleteTodo(state, action) => {
                state.tasks = state.tasks.filter((task) => task.id !==action.payload);
            },
        },
    });

    export const { addTodo, deleteTodo} = todoSlice.actions;

    export default todoSlice.reducer;
```

    The above code defines a slice named 'todoSlice', with an 
'initialState' object that contains an empty array of tasks.

    The 'reducers' object defines two reducer functions: 'addTask' and 
'deleteTask'. 'addTask' pushes a new task object into the 'tasks' array, 
and 'deleteTask' removes a task from the 'tasks' array based on its 
'id' property.

    The 'createSlice' function automatically generates action creators
and action types based on the names of the reducer functions you 
provide. So you don't have to define the action creators yourself
manually.

    The 'export' statement exports the generated action creators,
which can be used in other parts of your app to dispatch actions
to the slice.

    And finally, the 'todoSlice.reducer' function handles all
actions automatically generated based on the reducer objects provided
to the 'createSlice' function. By exporting it as the default, you
can combine it with other reducers in your app to create a complete
Redux store.

##### HOW TO SET UP REDUX STORE #####

    Create a Redux store is much simpler with Redux Toolkit.

    The most basic way to create a store is to use the 
'configureStore()' function, which automatically generates a root 
reducer for you by combining all the reducers defined in your app.

    To create a store for the app, add a file named 'src/store.js'
and add the following code:

```js
    import { configureStore } from '@reduxjs/toolkit'
    import todoReducer from './todoSlice'

    const store = configureStore({
        reducer: {
            todo:todoReducer,,
        },
    }),

    export default store;
```

    In this example, we first import the 'configureStore' function 
form the '@reduxjs/toolkit' package, and the 'todoReducer' function
from a separate file.

    Then, we create a 'store' object by calling 'configureStore' and
passing it an object with a 'reducer' property. The 'reducer' 
property is an object that maps reducer slice names to their 
corresponding reducer function. In this case, we have one reducer
slice called 'todo', and its corresponding reducer function is 
'todoReducer'.

    Finally, we export the 'store' object so that it can be imported
and used in other parts of the app.

##### HOW TO PROVIDE THE REDUX STORE TO REACT #####

    To make your Redux store available to the React components is your
app, import the Provider component from the 'react-redux' library and
wrap your root component (usually <App />) with it.

    The Provider component takes in the store as a prop and passes
it down to all the child components that need to access to it.

```js
    import React from 'react'
    import ReactDOM from 'react-dom'
    import App from './App.jsx'
    import './index.css'

    import store from './store.js'
    impor { Provider } from 'react-redux';

    ReactDOM.createRoot(document.getElementById('root')).render(
        <React.StrictMode> 
            <Provider store = {store}>
                <App />
            </Provider>
        </React.StrictMode>
    );
```

##### CREATE COMPONENTS #####

    You can now create React components such as 'Task.jsx' and 
'TaskList.jsx' that use the 'useSelector' hook to access the current
state from the store. Similarly, you can use the 'useDispatch()' hook
to dispatch actions to update the store, just as you did in plain Redux.

    You should now have the same app as before with a few updates from
Redux Toolkit and a lot less code to maintain.

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
    const firstBook = {     <------
        author:"JRR Tolkien",   <------
        title:"Silmarillion"
    };
    const secondBook = {      <------
        author:"J Rowling",      <------
        title:"Harry Potter"
    }

    const BookList = () => {
        return (
            <section className = "className">
                <Book
                    author = {firstBook.author}    <------
                    title = {firstBook.title}
                />
                <Book
                    author = {srcondBook.author}    <------ 
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
- Accepts id as an argument and finds the book
- Pass function down to Book Component and invoke on the button click
- In the Book Component destructure id and function 
- Invoke the function when user clicks the button, pass the id
- The goal: you should see the same book in the console

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

##### useState BASICS #####

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

##### GENERAL RULES OF HOOKS #####

- starts with 'use' 
- component must be uppercase
- invoke inside function/component body
- don't call hooks conditionally
- set functions don't update state immediately 

##### useState with Array #####

##### useState WITH OBJECT #####

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

##### useEffect BASICS #####

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

##### EXTRAS #####

- redux devtools
- combine reducers

##### REACT-REDUX #####

connects our app to redux

##### SETUP STORE #####

- create store.js

```js
    import {configureStore} from "reduxjs/toolkit";

    export const store = configureStore({
        reducer:{},
    });
```

##### SETUP PROVIDER #####

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

##### SETUP CART SLICE #####

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

##### FIRST REDUCER #####

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

```js
    import React from "react"
    import { useDispatch, useSelector} from 'react-redux';

    const CartContainer = () => {
        const dispatch = useDispatch();

        return (
            <button
                onClick = {() => {
                    dispatch(clearCart());
                }}
            >ClearCart
            </button>
        )
    };

    export default CartContainer;
```

##### REMOVE, INCREASE, DECREASE #####

- cartSlice.js

```js
    import { createSlice } from "@reduxjs/toolkit";
    import cartItems from "./.../"

    const initialState = {
        cartItems: [],
        amount:0,
        total:0,
        isLoading:true
    };

    const cartSlice = createSlice({
        name:'cart',
        initialState,
        reducers: {
            clearCart: (state) => {
                state.cartItems = [];
            },
            removeItem: (state, action) => {
                const itemId = action.payload;
                state.cartItems = state.cartItems.filter((item) =>
                item.id !== itemId);
            },
            increase: (state, {payload}) => {
                const cartItem = state.cartItems.find((item) => 
                item.id === payload.id);
                cartItem.amount = cartItem.amount + 1;
            },
            decrease: (state, {payload}) => {
                const cartItem = state.cartItems.find((item) => 
                item.id === payload.id);
                cartItem.amount = cartItem.amount - 1;        
            },
            calculateTotals:(state) => {
                let amount = 0;
                let total = 0;
                state.cartItem.forEach((item) => {
                    amount += item.amount;
                    total += item.amount * item.price;
                });
                state.amount = amount;
                state.total = total;
            };        
        };
    });
```

##### MODAL #####

- create components/Modal.js

```js
    const Modal = () => {
        return (
            <aside>
                <div>
                    <h4>Remove</h4>
                    <div>
                        <button 
                            type='button'
                            onClick = (()=> {
                                dispatch(clearCart());
                                dispatch(closeModal());
                            })
                        >
                            Confirm
                        </button>   
                        <button 
                            type='button'
                            onClick = (()=> {
                                dispatch(closeModal());
                            })
                        >
                            Cancel
                        </button>   
                    </div>
                </div>
            </aside>
        )
    }
```

- App.js

```js
    return (
        ...
        <Modal />
        ...
    )
```

##### MODAL SLICE #####

- create features/modalSlice.js

```js
    import { createSlice } from '@reduxjs/toolkit';
    const initialState = {
        isOpen:false,
    };

    const modalSlice = createSlice({
        name:'modal',
        initialState,
        reducers: {
            openModal: (state, action) => {
                state.isOpen = true;
            }, 
            closeModal: (state, action) => {
                state.isOpen = false;
        },
    });

    export const {openModal, closeModal} = modalSlice.actions;
    export default modalSlice.reducer;
```

-App.js

```js
    const { isOpen } = useSelector((state) => state.modal)

    return (
        <main>
            {isOpen && <Modal />}
            ....
        </main>
    )
```

##### ASYNC FUNCTIONALITY WITH createAsyncThunk #####

- action type
- callback function
- lifecycle actions

```js
    import { createSlice, createAsyncThunk } from "@reduxjs/toolkt"

    const url = "some url"

    export const getCartItem = createAsyncThunk('cart/getCartItem', () => {
        return fetch(url)
            .then((resp) => resp.json())
            .catch((err) => console.log(error));
    })
```

##### OPTIONS #####

```sh
    npm install axios
```

- cartSlice.js

```js
    export const getCartItems = createAsyncThunk(
        'cart/getCartItems',
        async (_, thunkAPI) => {
            try {
                console.log(thunkAPI.getState());

                const resp = await axios(url);
                return resp;
            } catch (error) {
                return thumkAPI.rejectWithValue('There was an error...');
            };
        };
    );
```




##### SCRIMBA REDUX #####

##### REDUCERS #####    

```js
    import { createStore } from 'redux'

    const house = {
        type:'condo',
        rooms:[],
        doorsOpen: {
            backDoor:false,
            frontDoor:true
        }
    }

    const reducer = (state = house, action) => {
        return state;
    }

    const store = createStore();

    export default store;
```

##### ACTIONS #####

```js
    import { createStore } from 'redux'

    const house = {
        type:'condo',
        rooms:[],
        doorsOpen: {
            backDoor:false,
            frontDoor:true
        }
    }

    const reducer = (state = house, action) => {
        if(action.type === "ADD_ROOM") {
            return Object.assign({}, state, {
                rooms:state.rooms.concat(action.payload)
            })
        }
        return state;
    }

    const store = createStore();

    export default store;
```

##### USING REDUX WITH REACT #####

    To connect React and Redux we should use <Provider>

- index.js

```js
    import React from 'react'
    import ReactDOM from 'react-dom'
    import App from './App'

    ///////////////////////////////////////
    import { Provider } from 'react-redux'
    ///////////////////////////////////////
    import store from './store/index'

    ReactDOM.render(
    ///////////////////////////////////////
    <Provider store={store}>
        <App />
    </Provider>, 
    //////////////////////////////////////
    document.getElementById('root')
)
```

- App.js

```js
    import React from 'react'
    import { useSelector } from 'react-redux'

    function App() {
        const rooms = useSelector(state => state.rooms)
        return (
            <>
                <div className="home-image"></div>
                <h1>Let's learn about homes!</h1>
                <ul>
                    {rooms.map(room => (
                        <li key={room.id}>{room.type}</li>
                    ))}
                </ul>
            </>
        )
    }

    export default App
```

##### DISPATCHING ACTIONS THROUGH COMPONENTS #####

- App.js 

```js
    import React, { useState } from 'react'
import { useSelector, useDispatch } from 'react-redux'

function App() {
    const rooms = useSelector(state => state.rooms)
    const dispatch = useDispatch()
    const [roomId, setRoomId] = useState(4)
    const [value, setValue] = useState('')
    
    const handleSubmit = (event) => {
        event.preventDefault()
        setRoomId({ roomId: roomId + 1 })
        dispatch({
            type: 'ADD_ROOM',
            payload: { id: roomId, type: value}
        })
    }
    
    return (
        <>
            <div className="home-image"></div>
            <h1>Let's learn about homes!</h1>
            <form onSubmit={handleSubmit}>
                <input 
                    type="text" 
                    value={value}
                    onChange={event => setValue(event.target.value)}
                />
                <input type="submit" value="Add a room"/>
            </form>
            <ul>
                {rooms.map(room => (
                    <li key={room.id}>{room.id}{room.type}</li>
                ))}
            </ul>
        </>
    )
}

export default App
```

- index.js

```js
    import { createStore } from 'redux'

const house = {
    type: 'condo',
    rooms: [
        {id: 1, type: 'Living Room'},
        {id: 2, type: 'Dining Room'},
        {id: 3, type: 'Bathroom'}
    ],
    doorsOpen: {
        backDoor: false,
        frontDoor: true
    }
}

const reducer = (state = house, action) => {
    if (action.type === 'ADD_ROOM') {
        return Object.assign({}, state, {
            rooms: state.rooms.concat(action.payload)
        })
    }
    return state
}

const store = createStore(reducer)

export default store

```

##### WORKING WITH ASYNC DATA #####

- index.js

```js
    import { createStore } from 'redux'

const house = {
    type: 'condo',
    rooms: [
        {id: 1, type: 'Living Room'},
        {id: 2, type: 'Dining Room'},
        {id: 3, type: 'Bathroom'}
    ],
    doorsOpen: {
        backDoor: false,
        frontDoor: true
    },
    ///////////////////////
    temp: 'Calculating...'
    //////////////////////
}

const reducer = (state = house, action) => {
    if (action.type === 'ADD_ROOM') {
        return Object.assign({}, state, {
            rooms: state.rooms.concat(action.payload)
        })
    }
    
    /////////////////////////////////////
    if (action.type === 'GET_TEMP') {
        return Object.assign({}, state, {
            temp: action.payload
        })
    }
    ////////////////////////////////////
    return state
}

const store = createStore(reducer)

export default store

```

- App.js

```js
    import React, { useEffect } from 'react'
import { useSelector, useDispatch } from 'react-redux'

function App() {
    const dispatch = useDispatch()
    const { rooms, temp } = useSelector(
        state => ({
            rooms: state.rooms,
             {/*////////////////////////////////////////*/}
            temp: state.temp
            {/*////////////////////////////////////////*/}
        })
    )
    {/*//////////////////////////////////////////////////////////////////////*/}
    useEffect(() => {
        fetch('https://api.oceandrivers.com:443/v1.0/getWeatherDisplay/cnarenal/?period=latestdata')
            .then(response => response.json())
            .then(json => {
                console.log(json)
                dispatch({
                    type: 'GET_TEMP',
                    payload: json.TEMPERATURE
                })
            })
    }, [])
    {/*/////////////////////////////////////////////////////////////////////*/}
    
    return (
        <>
            <div className="home-image"></div>
            <h1>Let's learn about homes!</h1>
            <ul>
                {rooms.map(room => (
                    <li key={room.id}>{room.type}</li>
                ))}
            </ul>
            {/*///////////////////////////////////////////*/}
            <p>The temperature outside is: {temp}</p>
            {/*///////////////////////////////////////////*/}
        </>
    )
}

export default App
```
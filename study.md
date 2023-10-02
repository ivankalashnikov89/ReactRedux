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

    const App = () => {
        return "Hello React";
    }

```
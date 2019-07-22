## Intro to promises
* There are two types of operations:
  * ` Synchronous ` **operations**  
  is executed immediately with no delay.
  
  * ` Asynchronous ` **operations**  
  is executed after some delay which depends on the operation itself and other factors.

* A ` Promise ` is a type of object that is used to initiate **asynchronous operations** and let you associate handlers with these operations.

* Once a ` Promise ` object is instantiated, it can be in one of these **states**:
  * **pending** which is the initial state and indicates that the **promise** is **yet under operation**.
  
  * **Fulfilled** which indicates that the **promise** is **resolved** or completed with **success** and **returned** a **value**.
  
  * **Rejected** which indicates that the **promise** is **completed with failure** and **returned** an **error**.

* A **promise handler** has one or both of these responsibilities:
  * **consumes** a returned **value** on **success**.
  
  * **handles** an **error** on **failure**.

#### Promise Syntax
* The ` Promise ` constructor accepts as arguments a function named **executor**.
```js
new Promise( function( resolve, reject ) { ... } );
```

* The **executor** function accepts:
  * one **mandatory argument** named **resolve** which is a function you call whenever you decide that the **promise** should turns into **fulfilled** with an **optional return value**.
  
  * one **optional argument** named **reject** which is a function you call whenever you decide that the **promise** should turns into **rejected** with an **error object**.

* **Hint**: The **executor** function starts executing as soon as you pass it for a ` Promise ` constructor immediately even before the ` Promise ` constructor returns the ` Promise ` object.  

* Example:
```js
const delayPromise = new Promise( function( resolve ) {
    setTimeout( function() {  resolve( "3 Seconds passed" ); } , 3000 );
    } );
```  
  * Here we instantiated a ` Promise ` object passing the **executor** function as anonymous.
  
  * The **executor** function accepts arguments the ` resolve ` method(As we said passing the ` reject ` method is optional).
  
  * ` setTimeout ` is a function which executes a given function after a certain amount of time. ` setTimeout` accepts as arguments the **function** to be executed and the **certain time**.
  
  * We could reduce this code using the **arrow functions**:
  ```js
  const delayPromise = new Promise( resolve => {
      setTimeout( () =>  resolve( "3 Seconds passed" ) , 3000 );
      } );
  ```

* Now let's make the **delay promise** more reusable by defining a ` function ` that returns a **delay** ` Promise ` and a given number of **seconds** passed as argument(**closure**).
```js
const delay = seconds => {
    return new Promise( ( resolve, reject ) => {
        if ( typeof seconds !== 'number' ) {
            reject( new Error( "Not a number" ) );
        }
        setTimeout( () =>  resolve( `${seconds} Seconds passed` ) , seconds * 1000 );
        } );
};
```

#### Handler Syntax
* To handle a **promise** we have two methods ` then `, ` catch `:
  * ` catch ` is used to handle **Rejected** state(**errors**).
  ```js
  delay('3').catch( error => console.error( error ) );  // Error: Not a number
  ```
  Hint: The **promise** is turned into **Rejected** state **only If** you did not catch the error. If you did by declaring **onRejection** method like what we did just now the **promise** is turned into **Resolved** state by catching errors and handling them.
  
  * ` then ` is used in two ways:
    * to handle **Resolved** state(**success**) only.
    ```js
    delay(3).then( message => console.info( message ) );  // 3 seconds passed
    ```
    
    * to handle both **Resolved** and **Rejected** states.
    ```js
    delay(1).then( message => console.info( message ), error => console.error( error ) );
    // 1 seconds passed
    ```

* ` then ` and ` catch ` methods return a ` Promise ` object which means you can **chain** more than one ` then ` functions.
```js
delay(3).then( message => message.toUpperCase() ).then( message => console.info( message ) );
// 3 SECONDS PASSED
```

* The ` Promise ` returned by ` then ` can be in one of its three states:
  * **Pending** when the handler function returns another **pending** ` Promise `.
  
  * **Resolved** when the handler function either returns an already **resolved** ` Promise `, returns a value(the returned ` Promise ` gets resolved  with the returned value as its value), or returns nothing(the returned ` Promise ` gets resolved with its valued set to ` undefined `).
  
  * **Rejected** when the handler function either **throws an error** or returns an already **rejected** ` Promise `.


## Building promises
* Now It's time to see our first example that calls an **API** and handles the response. We are gonna call an **API** that returns a list of **Astronauts**.
  1. Let's create our Promise
  ```js
  const getSpacePeople = () => {
      return new Promise( ( resolve, reject ) => {
          // Till now we just created a function that returns a Promise
          // Prepare the API's URL
          const URL = "http://api.open-notify.org/astros.Json";
          // Construct a request
          const REQUEST = new XMLHttpRequest();
          // Open the request and Specify the request method and the URL
          REQUEST.open( 'GET', URL );  // Since we will just read data, the request method is 'GET'
          // Specify what happens if the request is loaded successfully
          REQUEST.onload = () => {
              // check if the returned status is OK(200)
              if ( REQUEST.status === 200 ) {
                  // Parse the JSON object  
                  let parsedJSON = JSON.parse( REQUEST.response );
                  // resolve with value = parsedJSON
                  resolve( parsedJSON );
              }
          };
          // Specify what happens if an error occurs while loading
          REQUEST.onerror = error => reject( error );
          // Send the request
          REQUEST.send();
          } );
  };
  ```
  
  2. Let's handle our promise
  ```js
  /* Define onFulfillment and onRejection methods and use the then method to attach them to our Promise */
  getSpacePeople().then(
      response => console.info( response ),
      error => console.error( error )
      );
  ```
    * **Tip**: This line of code:
    ```js
    getSpacePeople().then(
        response => console.info( response ),
        error => console.error( error )
        );
    ```
    can be simplified to this:
    ```js
    getSpacePeople().then( console.info, console.error );
    ```
    Instead of providing **anonymous functions** for **onFulfillment** and **onRejection** in which we execute one function which is ` console.info `, ` console.error ` respectively, we provide a **reference** to each function to be called.

* It seems that this **URL** in the example does not work so far. For testing I used this URL 'https://jsonplaceholder.typicode.com/posts' and the script works.


## Loading data with fetch
* The **dash** sign(**-**) is known as **hyphen**.

* ` fetch ` function provides a **very simple** way to get data from APIs.

* ` fetch ` reduces much of the work that was done using ` XMLHttpRequest `.

* ` fetch ` just takes as arguments the **path to the resource you wanna get** or simply the **URL** and returns the **response**. This is the case when you want to make a ` GET ` request which is the default method. If you wanna make a request of another method like ` POST ` for example, you can pass ` fetch ` in the second argument a configuration object with ` method ` attribute and other attributes if you wish to set ` body `, ` headers ` and this stuff.

* The above ` getSpacePeople ` function can be simplified using ` fetch ` to this:
```js
const getSpacePeople = () => {
    const URL = "https://jsonplaceholder.typicode.com/posts";
    return fetch( URL ).then( response => response.json() );
}
```
Just like that? Yes, just like that.

* ` fetch ` returns a ` Promise ` object whose value is the ` Response ` to that request.

* On getting the ` Response `, we have a bunch of methods that we can use to deal with the content inside it. One of those methods is what we have just used which is ` json ` method which parses the ` Response ` to a JSON.

#### map
* ` map ` is a ` method ` used to loop through an array of objects executing a given operation on each one and returns a list of the results.
```js
var squares = [4, 9, 16, 25, 36];
var numbers = squares.map( Math.sqrt );
console.log( numbers );  // [2, 3, 4, 5, 6]
```
```js
function Student(name, age) {
    this.name = name;
    this.age = age;
}
var students = [new Student( "Hossam", 22 ), new Student( "Mossab", 23 ), new Student("Ayman", 21)];
var studentsNames = students.map( student => student.name );
console.log( studentsNames );  // ["Hossam", "Mossab", "Ayman"]
```


## Async and await
* ` async ` and ` await ` keywords are a new syntax used to deal with **asynchronous operations** in a **synchronous** way.

* ` async ` keyword is used before ` function ` keyword to indicate that the function is **asynchronous**.

* ` await ` is used before a ` Promise ` or a value generated by a ` Promise ` to say "Wait here till the ` Promise ` **executor** is done before you go any where".

* Let's see a normal ` function `:
```js
const delay = function( seconds ) {
    return new Promise( resolve => setTimeout(resolve, seconds * 1000) );
};
// normal function
(function countToThree() {
    console.log( "Zero Seconds" );
    delay(1);
    console.log( "One Seconds" );
    delay(1);
    console.log( "Two Seconds" );
    delay(1);
    console.log( "Three Seconds" );
}());
```  
This code will output:  
Zero Seconds  
One  Seconds  
Two Seconds  
Three Seconds  
but with no delays all at once.

* Now let's put some delay inside the normal ` function ` to turn it into **asynchronous** ` function `.
```js
const delay = function( seconds ) {
    return new Promise( resolve => setTimeout(resolve, seconds * 1000) );
};
// asynchronous function
(async function countToThree() {
    console.log( "Zero Seconds" );
    await delay(1);
    console.log( "One Seconds" );
    await delay(1);
    console.log( "Two Seconds" );
    await delay(1);
    console.log( "Three Seconds" );
}());
```
This will have the same output as the previous except that it executes with the delays we need.

* Let's see another example:
```js
const returnAfterDelay = ( seconds ) => {
    return new Promise( resolve =>
       setTimeout( () => resolve( seconds ) , seconds * 1000)
       );
};
( async() => {
    let x = returnAfterDelay( 2 );
    let y = returnAfterDelay( 4 );
    console.log( `We waited ${await x + await y} seconds` )
    } )();
```
Notice here we used the ` await ` keyword before the variable that will hold the value generated by a ` Promise ` not before the ` Promise ` itself.

* **Tip**: When defining an **IIFE**(Immediate invoked function expression):
  * If we are using normal syntax we can put the parenthesis which indicates calling the function before or after the last **)**. So both of these work:
  ```js
  ( function() {
      console.log( "A" );
      } () );
  ```
  ```js
  ( function() {
      console.log( "A" );
      } ) ();
  ```
  
  * However, if you decided to use **arrow function** you must put those parenthesis after the last **)**.
    * This does work.
    ```js
    ( () => {
        console.log( "A" );
        } ) ();
    ```
    * This does not work.
    ```js
    ( () => {
        console.log( "A" );
        } () ) ;  // Syntax error
    ```


## Async with fetch
* ` async ` can be used with ` fetch ` to do all the work inside one function with no need for defining a function that returns a ` Promise ` and to handle that ` Promise ` and all of that headache.

* Lets get my followers on Github which may be zero:
```js
( async function getFollowers( githubUserName ) {
    try {
      const URL = `https://api.github.com/users/${githubUserName}/followers`;
      var response = await fetch(URL);
      var json = await response.json();
      var followersUserNames = json.map( follower => follower.login )
      console.log( followersUserNames );
    } catch ( e ) {
        console.error( "Error fetching followers", e )
    }
} ) ("hossamnasser938");  // ["Ahm7dKhalifa", "oforomar", "GergesBernaba1", "mmousa8189"]
```

* Now let's play around with the previous script and make it normal(not asynchronous).
```js
( function getFollowers( githubUserName ) {
    try {
      const URL = `https://api.github.com/users/${githubUserName}/followers`;
      var response = fetch(URL);
      var json = response.json();
      var followersUserNames = json.map( follower => follower.login )
      console.log( followersUserNames );
    } catch ( e ) {
        console.error( "Error fetching followers", e )
    }
} ) ("hossamnasser938");  // Error fetching followers TypeError: response.json is not a function
```
The error in line 5. Since ` response ` is not returned yet it has no method called ` json `.


## Chapter quiz
1. What is the purpose of promises?  
to help developers manage asynchronous behavior in JavaScript.
2. How do you return a promise object?  
use return new promise()
3. What do you pass in to the fetch function?  
a url that you want to fetch from.
4. How can you use async/await in Node.js?  
Use node version 8.
5. How do you make fetch asynchronous?  
You can't since Fetch is already asynchronous.

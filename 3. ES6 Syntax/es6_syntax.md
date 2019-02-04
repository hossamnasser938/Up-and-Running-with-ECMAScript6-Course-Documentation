## Let Keyword
* Before ECMASCript 6 was released, we had only one way to declare a variable which is using the ` var ` keyword.
* ECMAScript came out with 2 new ways to declare a variable which are using the keywords ` let ` and ` const
`.

* Let's now differentiate between ` var ` and ` let `.
    * ` var ` is scoped to the nearest function.
    * ` let ` is scoped to the nearest enclosing block.

* There are 3 different situations that help us know the difference between ` var ` and ` let `.
    * **Global**  
    When declaring variables **globally** outside any function in the script body, ` var ` and ` let ` behave the same way since the nearest function and the nearest enclosing block is the same.
    ```
    <script type="text/javascript">
        var i = 1;
        let j = 2;
    </script>
    ```
    You cannot find any difference here between ` i ` and ` j ` except for one. The exception is that the variable ` i ` declared using ` var ` is added to the global ` window ` object. However, the variable ` j ` declared using ` let ` is not.
    ```
    console.log(window.i);  // 1
    console.log(window.j);  // undefined
    ```
    * **Function**
    When declaring variables **within a function**, ` var ` and ` let ` behaves the same way since the nearest function and the nearest enclosing block is that function.
    ```
    function x() {
        var v = "hossam";
        let l = "ismail";
        console.log( v );  // hossam
        console.log( l );  // ismail
    }
    x();
    console.log( v );  // undefined
    console.log( l );  // undefined
    ```
    * **Block**
    Here is the difference. When declaring variables inside a **small scope** like an **if statement** or **for loop**, there is a big difference between ` var ` and ` let `. In this case variables declared using ` var ` is accessible inside this small block as well as the rest of the function. However, variables declared using ` let ` is accessible only in this small block and not accessible in the rest of the function.
        * ` if ` statement
        ```
        function x() {
            if ( true ) {
                var v = "hossam";
                let l = "ismail";
                console.log( v );  // hossam
                console.log( l );  // ismail
            }
            console.log( v );  // hossam
            console.log( l );  // undefined
        }
        x();
        ```
        Another example that makes the difference significant:  
            * using ` let `.
            ```
            function x() {
                var v = "hossam";
                if ( true ) {
                    let v = "ismail"
                    console.log( v );  // ismail
                }
                console.log( v );  // hossam
            }
            x();
            ```
            Here we have two variables with the same name but each of which has a different scope. One scoped to only the ` if ` statement while the other is scoped to the entire ` function `.
            * using ` var `.
            ```
            function x() {
                var v = "hossam";
                if ( true ) {
                    var v = "ismail"
                    console.log( v );  // ismail
                }
                console.log( v );  // ismail
            }
            x();
            ```
            Here we have one variable with the same name and the same scope since ` if ` statement is not considered a scope with ` var `.

        * ` for ` loop  
        When working with loops, defining the iterator ` i ` using ` var ` is a bit different from declaring it using ` let `. In the case of ` var ` the iterator is not accessible only in the ` for ` loop but also in the rest of the ` function ` wrapping this ` for ` loop which means it is only one variable called ` i ` in the entire scope wrapping this ` for ` loop. However, in the case of ` let ` the variable is accessible only in the ` for ` loop which means that in each iteration of the loop a new variable is created with name ` i ` and it takes its value from the previous iteration(where we left off).  
        Let's walk through this example to understand the difference well.
        ```
        var a = [];
        for ( var i = 0; i < 10; i++ ) {
            a[i] = function() {
                console.log( i );
            };
        }
        ```
        Here we declared an array and looped 10 times assigning each element of the array a function that logs the value of i at each iteration. Although we expect each item in the array holds a function that logs a different i based on its index in the array, we find that all functions within the array logs the same i value which is 10. This is because i is declared using ` var ` so it is only one variable in the entire script scope that will eventually hold one value at the end of the loop which is 10.
        ```
        a[0]();  // 10
        a[3]();  // 10
        a[7]();  // 10
        ```
        However if we used ` let ` instead when declaring i, we will have for each iteration a new variable tight to the ` for ` loop scope and has a unique value.
        ```
        var a = [];
        for ( let i = 0; i < 10; i++ ) {
            a[i] = function() {
                console.log( i );
            };
        }
        a[0]();  // 0
        a[3]();  // 3
        a[7]();  // 7
        ```
        Is there any other way to achieve this output while still using ` var `?  
        Yes, there is. Remember we want each function to have a unique value for i which means a function wrapped with a lexical environment. It is **closure**.
        ```
        function logI( i ) {
            return function() {
                console.log( i );
            }
        }
        var a = [];
        for ( var i = 0; i < 10; i++ ) {
            a[i] = logI( i );
        }
        a[0]();  // 0
        a[3]();  // 3
        a[7]();  // 7
        ```

* **References**:
    * [ES6: var, let and const — The battle between function scope and block scope](https://www.deadcoderising.com/2017-04-11-es6-var-let-and-const-the-battle-between-function-scope-and-block-scope/).
    * [What's the difference between using “let” and “var” to declare a variable in JavaScript?](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var-to-declare-a-variable-in-jav).
    * [let vs var: scopes in for-loop [duplicate]](https://stackoverflow.com/questions/38801257/let-vs-var-scopes-in-for-loop)

* Some tips from the **Boxes** example:
    * to specify **CSS rule** for a tag in another tag simply reference the outer followed by > followed by inner.
    ```
    section > div {
        width: 100px;
        height: 100px;
        background-color: red;
        float: left;
        margin: 3px;
        cursor: pointer;
    }
    ```
    We have ` div ` tags inside ` section ` tag.
    * to create an HTML element use the ` createElement ` method and pass the tag needed.
    ```
    var div = document.createElement("div");
    ```
    * to append that element to another node, use the method ` appendChild ` on the node you wanna append the new element to.
    * the method ` getElementsByTagName ` returns an array of the returned elements that matches the given tag so you can access each one using bracket notation.
    ```
    document.getElementsByTagName('section')[0].appendChild(div);
    ```

## Const Keyword
* ` const ` is another way to **declare variables**.
* Once ` const ` is **initialized**, it can not be **reassigned** neither **redeclared**. It is used to declare things that can not be changed such as maths constants(ex: PI), birthday, and more.
```
const PI = 3.14;
PI = 3.15;  // Error: reassignment
```
```
const PI = 3.14;
var PI;  // Error: redeclaring
PI = 3.15;
```
* ` const ` must be **initialized** in declaration statement.
```
const PI;  // Error: missing initialization
PI = 3.14;  
```
* ` const ` has a **block scope** like ` let `.
* A block that contains `const` can not contain a variable or even a function with the same name.
```
const PI = 3.14;
function PI() {  // Error: redeclaring
    console.log(PI);
}
```
* If ` const ` is assigned an **object reference**, this reference can not change. However, **properties** in this object can be reassigned with no problem.


## Template strings
* This thing we use to surround code in .md files is called **backticks**.
* **Template strings** is a new feature in **ES6** which reduces the headache of concatenating strings with variables, putting necessary spaces between a string and a variable, putting line breaks and all that stuff.
* consider this old **ES5**:
```
function sayHello( firstName ) {
    console.log( "Hello " + firstName + "\nWelcome on the board" );
}
```
Using **template stringd**:
```
function sayHello( firstName ) {
    console.log(`Hello ${firstName} Welcome on Board`);
}
```
You just wrap your string in **backticks** and surround any variable with **${}**.
* Also you can use the arithmetic operator **+** not the concatenation one in **${}**. Consider these differences.
```
let a = 1, b = 2;
console.log( "a + b = " + a + b );  // a + b = 12
```
```
let a = 1, b = 2;
console.log( "a + b = " + (a + b) );  // a + b = 3
```
```
let a = 1, b = 2;
console.log( `a + b = ${a + b}` );  // a + b = 3
```


## Spread operators
* **Spread operators** or **spread syntax**, as the name implies, is used to **spread** or **expand** the elements of an iterable such as an array or a string when these elements are required expanded like in function calls where the function accepts multiple arguments.
```
function sum( a, b, c ) {
  return a + b + c;
}
var numbers = [1, 2, 3];
var summation = sum( ...numbers );
console.log(summation);  // 6
```
* **spread syntax** can be used to copy an array or concatenate multiple arrays.
```
// copying array equivalent to arr.slice()
var arr = [1, 2, 3];
var a = [...arr];  
```
```
// concatenating multiple arrays
let menoufFriends = ["Mossab", "Ayman", "Ismail"];
let meniaFriends = ["Gamal", "Adham", "Abobakr"];
let allFriends = [...menoufFriends, ...meniaFriends];
console.log(allFriends);  // ["Mossab", "Ayman", "Ismail", "Gamal", "Adham", "Abobakr"]
```
* **spread syntax** can be used to copy an object or concatenate multiple objects.
    * copying an object
    ```
    // copying an object
    var obj = {name: "Hos", age: 22};
    var coppiedObj = {...obj};
    coppiedObj.name = "Ismail";
    console.log(obj);  // {name: "Hos", age: 22}
    console.log(coppiedObj);  // {name: "Ismail", age: 22}
    ```
    Notice this is not equivalent to the assignment(coppiedObj = obj) since assignment assigns the reference of the object. However, using the spread syntax makes an entire new object with a new reference. The difference is shown here:
    ```
    // assigning an object
    var obj = {name: "Hos", age: 22};
    var assignedObj = obj;
    assignedObj.name = "Ismail";
    console.log(obj);  // {name: "Ismail", age: 22}
    console.log(assignedObj);  // {name: "Ismail", age: 22}
    ```
    * conctenating objects
    ```
    // conctenating objects
    var obj1 = {name: "Hos", age: 22};
    var obj2 = {faculty: "FEE", year:"Forth"}
    var concObj = {...obj1, ...obj2};  // {name: "Hos", age: 22, faculty: "FEE", year:"Forth"}
    ```
* **spread syntax** can be used to expand a string into array of individual characters.
```
var x = "Hello Hos."
var arr = [...x];  // ["H", "e", "l", "l", "o", " ", "H", "o", "s", "."]
```   

## Maps
* A **Map** is a type of object which contains **key-value** pairs. **Regular Objects** also contain **key-value** pairs so what is the difference?
    * In **Object** valid keys are strings and symbols only while in **Map** it can be a string, a number, an object, a function, any thing.
    * In **Object** keys are keys and values are values while in **Map** a key can be a value and a value can be a key.
    * **Object** is not iterable in the sense that you can not iterate through key-value pairs, instead you should do that manually while **Map** is iterable and the key-value pairs are ordered based on insertion(what is inserted first is stored first, what is inserted next is stored next and so on).
    * In **Object** there is no way to get how many properties in an object while **Map** has a ` size ` property which tells how many **key-value pairs** are there.

* **Syntax**
    * To create a **Map** use the Map constructor which can:
        * be **empty**
        ```
        var student = new Map();
        ```
        in this case you can use the ` set ` method to insert key-value pairs
        ```
        student.set( "name", "Hos" );
        student.set( "friends", ["Ayman", "Mossab", "Ismail"] );
        student.set( 22, "age" );
        ```
        Now you can use the ` get ` method to retreive the value of a certain key. We can not use the dot notation with **Map**.
        ```
        console.log( student.get( "name" ) );  // "Hos"
        console.log( student.get( "friends" ) );  // ["Ayman", "Mossab", "Ismail"]
        console.log( student.get( 22 ) );  // "age" notice here the key is a number
        ```
        * accept an array of key-value pairs(arrays of two values).
        ```
        var student = new Map( [
          ["name", "Hos"],
          ["friends", ["Ayman", "Mossab", "Ismail"]],
          [22, "age"]
          ] );
        ```
    * to get how many key-value pairs of a **Map**:
    ```
    console.log( student.size );  // 3
    ```
    * In **Map** We can loop through key-value pairs using ` forEach ` method.  
        * Loop through the values.
        ```
        student.forEach( function( element ) {
            console.log(element);
          } );
        ```
        * Loop through keys and values.
        ```
        student.forEach( function( value, key ) {
            console.log(`${key} : ${value}`);
          } );
        ```


## Sets
* A **set** is a collection of **unique** elements(any type: primitives or objects references) with no care about order.
* to declare a set use the ` Set ` constructor which can:
    * be **empty**
    ```
    var s = new Set();
    ```
    to add elements to the set.
    ```
    s.add("hos");
    s.add(22);
    s.add(["Ayman", "Mossab"]);
    ```
    * accept an iterable such as an array.
    ```
    var s = new Set( ["hos", 22, ["Ayman", "Mossab"]] );
    ```
* There are some useful methods that can be used with ` Sets `:
    * ` delete ` to delete an element. ` delete ` accepts as arguments the item to be deleted from the set and returns a boolean stating whether the element has been deleted successfully or not.
    ```
    console.log( s.delete(22) );  // true  
    ```
    * ` has ` to check whether the set contains a specific element or not. ` has ` accepts as arguments the element to be checked and returns a boolean stating whether the element exists or not.
    ```
    console.log( s.has("hos") );  // true
    ```
    * We can use the ` size ` property to get how many elements are there in a ` Set `.
    ```
    console.log( s.size );  // 2
    ```
* Remember that a **set** contains unique elements so if we passed to its constructor an array containing duplicate elements it will remove these duplicates.
```
var arr = [1, 2, 2, 3, 1];
var set = new Set(arr);
console.log( arr.length );  // 5
console.log( set.size );  // 3
```


## The for...of loop
* ` for...of ` is a new way to loop through iterables such as arrays, sets, maps, and strings.
    * **Strings**
    ```
    var name = "Hossam";
    for ( let letter of name ) {
        console.log( letter )
    }
    ```
    you get this output:  
    H  
    o  
    s  
    s  
    a  
    m  
    * **Arrays**
    ```
    var friends = ["Ayman", "Mossab", "Ismail"];
    for ( let friend of friends ) {
        console.log( friend );
    }
    ```
    you get this output:  
    Ayman  
    Mossab  
    Ismail  
    * **Sets**
    ```
    var friendSet = new Set(friends);
    for ( let friend of friendSet ) {
        console.log( friend );
    }
    ```
    You get the same output we got for array friends.
    * **Maps**
    ```
    var map = new Map([
      ["name", "Hos"],
      ["age", 22]
      ]);
    ```
        * Iterate through keys.
        ```
        for ( let key of map.keys() ) {
            console.log( key );
        }
        ```
        you get this output:  
        name  
        age  
        * Iterate through values.
        ```
        for ( let value of map.values() ) {
            console.log( value );
        }
        ```
        you get this output:  
        Hos  
        22  
        * Iterate through both keys and values.
        ```
        for  ( let element of map.entries() ) {
            console.log( element );
        }
        ```
        you get this output:  
        ["name", "Hos"]  
        ["age", 22]  
        or simply:
        ```
        for  ( let element of map ) {
            console.log( element );
        }
        ```
        you get the same previous output.


## Chapter quiz
1. Why might you use the 'let' keyword instead of 'var'?  
to enforce block scoping.
2. What happens if you try to reset a variable that has been declared as a constant?  
console error.
3. Are spaces recognized within a template string?  
Yes, any spaces or characters are recognized.
4. What punctuation makes up the spread operator?  
...

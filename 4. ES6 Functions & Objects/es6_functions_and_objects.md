## Default function parameters
The new feature "**Default function parameters**" has been explained at chapter 2("Transpiling ES6") in video "In-browser Babel transpiling".


## Enhancing object literals
* We have two ways of **creating objects** in JavaScript:
    * Using the **constructor**.
    ```
    var s = new Student("Hos", "Forth", "Good");
    ```
    ofcourse you have to define a ` function ` for the constructor ` Student `. It may seem like this:
    ```
    function Student( name, year, grade ) {
        this.name = name;
        this.year = year;
        this.grade = grade;
        this.describeStudent = function() {
            console.log( `Name: ${this.name}, Year: ${this.year}, Grade: ${this.grade}` );
        }
    }
    ```
    This way has the advantage of **reusage**. You can reuse this constructor to define as many objects as you need. Especially, You do not have to redefine methods each time you instantiate an object.
        * **Hint**: ` instanceof ` operator is used to check if an object is created from a given constructor. Notice that ` typeof ` operator is used to check the **type** of **primitive** variables.
        ```
        var s = new Student("Hos", "Forth", "Good");
        var id = 12345;
        console.log( s instanceof Student );  // true
        console.log( typeof id == "number" );  // true
        ```
    * Using what's called **Object Literals**.
    ```
    var s = {
        name : "Hos",
        year : "Forth",
        grade : "Good",
        describeStudent : function() {
            console.log( `Name: ${this.name}, Year: ${this.year}, Grade: ${this.grade}` );
        }
    };
    ```
    This way has the advantage of **simplicity** and used when you need to create an object rapidly especially when it does not have any methods or we will not create objects in the future with this skeleton.

* An example of an object is the ` Array ` object.
    * Literal ` Array `.
    ```
    var friends = ["Ayman", "Hasan", "Mossab"];
    ```
    * Using a **Constructor**.
    ```
    var friends = new Array( "Ayman", "Hasan", "Mossab" );
    ```
    the ` Array ` constructor can:
        * be **empty**.
        ```
        var arr = new Array();
        ```
        an empty array of length 0 is created.
        * **accept** the **elements** of the array.
        ```
        var friends = new Array( "Ayman", "Hasan", "Mossab" );
        ```
        an array of length 3 holding elements ["Ayman", "Hasan", "Mossab"] is created.
        * **accept** the **length** of the array.
        ```
        var friends = new Array( 3 );
        ```
        an empty array of length 3 is created. This case is identified when the ` Array ` constructor is given only **one** argument of `number` type.

* **ES6** has enhanced the way we use **Literal Objects**. Now no need to use the ` function ` keyword. Remember the ` Student ` literal object we created above. Now we can do it simply:
```
var s = {
    name : "Hos",
    year : "Forth",
    grade : "Good",
    describeStudent() {
        console.log( `Name: ${this.name}, Year: ${this.year}, Grade: ${this.grade}` );
    }
};
```

* Suppose we wanna define a ` function ` that repeats a pre-defined string a given number of times. Let the predefined string is **"meow"**. For example passing **3** to the function we should see **"meowmeowmeow"**. We have many ways to do that:
    * **Readable** but **much code**.
    ```
    function meow( times ) {
        let s = "";
        for ( let i = 0; i < times; i++ ) {
            s += "meow";
        }
        console.log( s );
    }
    ```
    * **Less code** but **weird**
    ```
    function meow( times ) {
        console.log( Array(times + 1).join( "meow" ) );
    }
    ```
        * **Hint**: **Array** can be called as a constructor ` new Array() ` as well as a function ` Array() ` with the same result. Let's discuss how [and possibly why] does this occur?
        * First let's see what happens when we call ` Student() ` as a **constructor** and as a **function**.
            * As a **constructor**  
            ```
            var s = new Student( "Mossab", "Forth", "Good" );
            ```
            It simply returns a new object that is ` instanceof ` Student. You actually returns nothing but JavaScript does this for you. ` this ` keyword inside the ` Student ` function when called as a constructor using ` new ` keyword refers to the newly instantiated object.
            * As a **function**  
            ```
            var s = Student( "Mossab", "Forth", "Good" );
            ```  
            In this case the ` student ` function returns nothing thus ` s ` is ` undefined `. What's worth than that is ` this ` inside ` Student ` function refers to the ` Window ` global object thus after executing this line you will get three variables defined globally: **name**, **year**, and **grade** as well as a function named **describeStudent**.
            ```
            console.log( name );  // "Mossab"
            ```
            Certainly you now feel like me. It's a problem, right? Let's see two ways to solve this problem:
                * **Prevent** using ` Student ` as a **function**  
                We can prevent that by defining ` Student ` **function** in ` strict ` **mode**. By doing this on calling ` Student ` as a function rather than a **constructor** you will get an error. We define a ` function ` or a whole **script** in **strict mode** by putting ` "use strict" ` in the first line.
                ```
                function Student( name, year, grade ) {
                    "use strict"
                    this.name = name;
                    this.year = year;
                    this.grade = grade;
                    this.describeStudent = function() {
                        console.log( `Name: ${this.name}, Year: ${this.year}, Grade: ${this.grade}` );
                    }
                }
                var s = Student( "Mossab", "Forth", "Good" );  // Error
                ```
                * **Handle** that and let calling ` Student ` as a **function** behaves as it were called as a **constructor**.
                ```
                function Student( name, year, grade ) {
                    "use strict"
                    if( this instanceof Student ) {
                        this.name = name;
                        this.year = year;
                        this.grade = grade;
                        this.describeStudent = function() {
                            console.log( `Name: ${this.name}, Year: ${this.year}, Grade: ${this.grade}` );
                        }
                    } else {
                        return new Student( name, year, grade );
                    }
                }
                var s = Student( "Mossab", "Forth", "Good" );
                console.log( s );
                /* Student {name: "Mossab", year: "Forth", grade: "Good", describeStudent: ƒ} */
                ```
                #### Modes in JavaScript
                * In JavaScript we have two different modes:
                1. **Sloppy mode**  
                The default mode which we are familiar with.
                2. **Strict mode**  
                Which as the name implies, it is more restricted than the **Sloppy mode** which means it puts some restrictions when you write code as well as when this code is executed. These restrictions allow writing safe code but with less flexibility than **Sloppy mode**. to switch to **Strict mode** put ` "use strict"; ` on top of your **script** or **function**.
                * Now let's rapidly see some differences between these modes:
                    * In **strict mode** variables must be explicitly declared.
                    ```
                    function sloppyFun() {
                        timp = 5;
                    }
                    sloppyFun();  // no problem
                    ```
                    ```
                    function strictFun() {
                        "use strict";
                        timp = 5;
                    }
                    strictFun(); // Reference error
                    ```
                    This helps to prevent typos since if you are in sloppy mode and you intend to assign a variable called **temp** and instead of typing:
                    ```
                    temp = 5;
                    ```
                    You accidentally typed:
                    ```
                    timp = 5;
                    ```
                    JavaScript will declare a global variable named **timp** and initialize it with value equals 5.
                    * In **strict mode** functions must be declared at top of the scope( function or script ).
                    ```
                    function sloppyFun() {
                        if ( true ) {
                            function x() {  // no problem
                                // ...
                            }
                        }
                    }
                    ```
                    ```
                    function strictFun() {
                        "use strict";
                        if ( true ) {
                            function x() {  // syntax error
                                // ...
                            }
                        }
                    }
                    ```
                    [This is what the documentation says. However, I tested it on Chrome and Mozilla and it workd with no syntax error].  
                    However, you can use a variable declaration and a function expression with no problem.
                    ```
                    function strictFun() {
                        "use strict";
                        if ( true ) {
                            var x = function() {  // no problem
                                // ...
                            }
                        }
                    }
                    ```
                    * In **strict mode** ` this ` is **undefined** in nonmethod functions[method functions are functions that are tight to a specific class or object]. However, in **sloppy mode** ` this ` is the global ` window ` object. This difference is what makes constructors defined in **strict mode** do not work as **functions**. Remember the ` Student ` **function** that we declared in **strict mode**. If we attempt to call it as a **function** since ` this ` is **undefined** it will generate error on executing:
                    ```
                    this.name = name;
                    ```
                    However if you call it as a constructor using ` new ` keyword ` this ` refers to the to-be-created object.
                * **References**:
                    * http://raganwald.com/2014/07/09/javascript-constructor-problem.html.
                    * https://stackoverflow.com/questions/5827008/whats-the-difference-between-array1-and-new-array1-in-javascript.
                    * Chapter 7 ("JavaScript Syntax") in the book Speaking JavaScript.  
    * A very simple(**less code**) and **readable** way is the new feature introduced by **ES6**. using the ` repeat ` method on strings.
    ```
    function meow( times ) {
        console.log( "meow".repeat( times ) );
    }
    ```


## Arrow functions
* **ES6** has provided us with an abbreviated form to define functions. It is the **Arrow function**.
* Let's remember the normal form:
```
var sayHello = function ( name ) {
    console.log( `Hello ${name}` );
}
```
* Let's see the new form:
```
var sayHello = ( name ) => console.log( `Hello ${name}` );
```
Even you can neglect the parenthesis surrounding ` name ` since it is only one argument but if there were more than one the parenthesis are mandatory.
```
var sayHello = name => console.log( `Hello ${name}` );
```
* **Arrow functions** also can be of multiple lines.
```
var sayHello = ( name ) => {
    console.log( `Hello ${name}` );
    console.log( "Hello ya Basha ");
}
```
* **Arrow functions** can ` return ` values without the ` return ` keyword only if the function is a one-line statement which is the ` return ` statement. If a function that returns something but not one line use the ` return ` keyword normally.
    * Normal way.
    ```
    var getWarmWelcome = function( name ) {
        return `Hello ya ${name}`;
    }
    ```
    * Arrow function
    ```
    var getWarmWelcome = ( name ) => `Hello ya ${name}`;
    ```


## Arrow functions and the 'this' scope
* **Arrow functions** can help in the problem of **scoping**.
* Let's see the **scoping** problem in an example.
```
var cat = {
    name: "Sam",
    actions: ["meow", "run"],
    printActions() {
        this.actions.forEach( function( action ) {
            var actionDescription = this.name + " can " + action;
            console.log( actionDescription );
            } );
    }
};
cat.printActions();
```
ofcourse you expect this output:  
Sam can meow  
Sam can run  
However you will get either:
    * an error.
    * this output:  
    can meow  
    can run  

* Where is the **problem**?  
The problem is with **scoping**.
    * inside the ` printAction ` method ` this ` successfully refers to the ` cat ` object. So this line:
    ```
    this.actions.forEach( function( action ) {
    ```
    has no problem. However, on **getting into** the **anonymous function** which is a parameter to the ` forEach ` method you lose the reference to the ` cat ` object. So the problem is in this line:
    ```
    var actionDescription = this.name + " can " + action;
    ```
    ` this ` now does not refer to the ` cat ` object. Surprisingly ` this ` in this line refers to the ` Window ` (what was referenced before getting into the ` cat ` literal object).

  * To solve this issue, we have different ways:
      * keep track of ` this ` in a variable before getting into the **anonymous function**.
      ```
      var cat = {
          name: "Sam",
          actions: ["meow", "run"],
          printActions() {
              let _this = this;
              this.actions.forEach( function( action ) {
                  var actionDescription = _this.name + " can " + action;
                  console.log( actionDescription );
                  } );
          }
      };
      cat.printActions();
      ```
      * ` bind ` ` this ` in the **anonymous function**.
      ```
      var cat = {
          name: "Sam",
          actions: ["meow", "run"],
          printActions() {
              this.actions.forEach( function( action ) {
                  var actionDescription = this.name + " can " + action;
                  console.log( actionDescription );
                  }.bind(this) );
          }
      };
      cat.printActions();
      ```  
  * Let's understand what is ` bind `?  
  ` bind ` lets us define a function that is wrapped with a specific ` this ` which we give as a parameter using ` bind `. Wherever you define that function, it has its own ` this `. Let's see an example to understand it well.
  ```
  var obj = {
      x: "some value",
      logX() {
          console.log(this.x);
      }
  }
  var normalMethod = obj.logX;
  var bindedMethod = obj.logX.bind(obj);
  normalMethod();  // undefined
  bindedMethod();  // some value
  ```
  Here we have two methods:
      * ` normalMethod `  
      in this case ` this ` refers to the ` Window ` and the ` Window ` does not have something called ` x `.
      * ` bindedMethod `  
      in this case ` this ` refers to ` obj ` since we **binded** it and ` obj ` has a property called ` x `.

  * Now let's see how **Arrow functions** solve this issue of **scoping**.
  ```
  var cat = {
      name: "Sam",
      actions: ["meow", "run"],
      printActions() {
          this.actions.forEach( action => {
              var actionDescription = this.name + " can " + action;
              console.log( actionDescription );
              } );
      }
  };
  cat.printActions();
  ```
  Now we do not have an **anonymous function** so ` this ` still refers to the ` cat ` object.


## Destructuring assignment
* **Destructuring assignment** is a new way to **retrieve** or **unpack** values from __**literal**__ **arrays** or **objects**.
* Let's assume that we have an array of elements ` arr ` and we wanna retrieve the first and third elements in variables to be used in further processing.
```
var arr = ['h', 'i', 'j', 'k', 'l', 'm'];
```
    * **Normal way**.
    ```
    var first = arr[0];
    var third = arr[2];
    ```    
    * **Destructuring assignment**
    ```
    var [first, , third] = arr;
    ```
* When using **destructuring assignment** with objects Take care of this:
    * This code does not work.
    ```
    {a, b} = {x: 5, y: 10};
    ```
    * Instead the assignment should wrapped in parenthesis. Why? have no idea.
    ```
    ({a, b} = {x: 5, y: 10});
    ```
* It seems that "Well, this is good but not very useful". No it is very useful. Let's see some examples(among them some useful use cases).
    * Destructuring an array to first, second, and **rest**.
    ```
    var [first, second, ...rest] = arr;
    console.log( first );  // 'h'
    console.log( second );  // 'i'
    console.log( rest );  // ['j', 'k', 'l', 'm']
    ```
    * Suppose that we have an object with many properties and we need to pass only 3 of those properties to a function.
        ```
        var student = {
            name: "Hos",
            age: 22,
            faculty: "FEE",
            year: 4,
            department: "Computer science",
            section: 2
        }
        ```
        * **Normal way**.
        ```
        function describeStudent( name, faculty, department ) {
            console.log( `${name} studies at ${faculty} in department ${department}` );
        }
        describeStudent( student.name, student.faculty, student.department );
        ```
        * **Destructuring assignment**.
        ```
        function describeStudent( {name, faculty, department} ) {
            console.log( `${name} studies at ${faculty} in department ${department}` );
        }
        describeStudent( student );  // very clean
        ```
    * **Swapping** two variables.
    ```
    var a = 2, b = 4;
    ```
    We could make this initialization this way:
    ```
    var [a, b] = [2, 4];
    ```
        * **Normal way**.
        ```
        var temp = a;
        a = b;
        b = temp;
        ```
        * **Destructuring assignment**.
        ```
        [a, b] = [b, a];
        ```
    * Accepting **multiple values returned** from a ` function ` in the form of an **array**.
    ```
    function f() {
        return [10, 20];
    }
    var [x, y] = f();
    console.log( x );  // 10
    console.log( y );  // 20
    ```


## Generators
* This feature requires a new **babel** script called **browser-polyfill**. Here is a link https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser-polyfill.min.js.
* **Generator functions** are functions that __generates__ **Generator objects**. It is identified by an **asterisk** after ` function ` keyword like this ` function* `
* What is different about this type of functions is that it can be paused(using the ` yield ` keyword) in the middle of execution and resumed as many times as you need.
* **Generator objects** are objects with 2 properties and 3 methods:
    * **properties**.
        * ` value ` the value generated by the **generator function**.
        * ` done ` which is a boolean indicating whether it is the last value and the generator function terminated or not.
    * **methods**
        * ` next() ` gets the **next** value specified by the next ` yield `.
        * ` return() ` gets the last value and terminate.
        * ` throw() ` throw an error.
* Let's see an example to see how it works. Suppose we wanna print the **English alphabets** one each second in order. Certainly what comes to your mind when I say **"each second"** is the ` setInterval ` method. However the question is how each time you print an alphabet in turns.
    ```
    var alphabets = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'];
    ```
    * We can keep track of the next letter to be printed.
    ```
    var nextIndex = 0;
    var interval = setInterval(logAnAlphabet, 1000);
    function logAnAlphabet() {
        if ( nextIndex < alphabets.length ) {
            console.log( alphabets[nextIndex++] );
        } else {
            console.log( "Finished" )
            clearInterval( interval );
        }
    }
    ```     
    * Using **Generators**
    ```
    var alphabetEachTime = (function*() {
        for ( let alphabet of alphabets ) {
            yield alphabet;
        }
        yield "Finished";
        clearInterval( interval );
    }());
    var interval = setInterval( () => {
		    let obj = alphabetEachTime.next();
		    if (!obj.done) {
			      console.log( obj.value);
		    }
    } , 1000 );
    ```
    Notice we have here a good use case for an **immediately invoked function expression**. We need to create a function and execute it at one step.

## Symbols
* You remember what a ` const ` is. Simply it is a variable that cannot be reassigned once it is declared and initiated. It lets us protect ourselves from, intentionally or not, changing things that should not be changed.
* Sometimes we need to achieve the same goal but with certain objects properties such as an ` id `. An ` id ` of an entity should not be changed. How can we achieve that?
    * If we created an object and assigned a property **called** ` id ` a value, we can, intentionally or not, change this value in the future when we should not.
    * ` Symbol ` comes here to solve this problem.
* A ` symbol ` is a new **primitive** data type introduced in **ES6**.
    * to create a variable of type ` symbol `, use the **factory** method ` Symbol ` with **s** capitalized. We now that a **factory method** is used to create things and is an alternative approach to **constructors**.
    ```
    const id = Symbol();
    ```  
    * What ` Symbol ` does is it returns a unique value each time. So if we used this value as an **identifier** to a property using the **bracket notation** we will have no way to change the value of that property except for using this unique value which means that when passing the object to somewhere in the project where a different programmer "x" coding there this programmer "x" has no way to change the value of this property unless you pass him this unique identifier.
    ```
    var student = {
        name: "Hos",
        age: 22
    };
    student[id] = 123456;
    ```
    * ` Symbol ` factory function **optionally** takes a value called **description** for debugging purposes.
    ```
    console.log( Symbol() == Symbol() );  // false because of they are unique values
    console.log( Symbol("foo") == Symbol("foo") );  // false for the same reason
    console.log( typeof Symbol() == typeof Symbol() ); // true because of them of type "symbol"
    console.log( typeof Symbol("foo") == typeof Symbol("foo") ); // true for the same reason
    ```
* ` symbol ` primitive types can be used to define a **customized iteration behavior** which we will see in the next section.


## Iterators
* We know generally what a **protocol** is. It is a set of **rules** defined to specify the way we deal with a certain thing.
* **ES6** has defined two new **protocols**: **Iterable** and **Iterator**. These protocols can be implemented by **objects** by a pre-defined way.

#### Iterable Protocol
* **Iterable protocol** is used to state that an object(which implements it) can be iterated as well as specify or customize some iteration behavior such as what values are looped over when using this object with ` for..of ` construct.
* **Built-in types** can be:
    * **iterable** with a default iteration behavior such as ` Array `, ` Map `.
    * **not iterable** such as ` Object `.
* For an object to be **iterable** it must define a method to specify its iteration behavior. As well as this method must be assigned to a given **key** or **property** named ` @@iterator ` which is the value of this constant ` Symbol.iterator `. In addition to that this method should return an object that implements the ` iterator ` protocol.
* If that is not yet clear, let's see how ` Array ` is **iterable**. If we created an object of ` Array ` and accessed a property identified by ` Symbol.iterator ` we get a method which defines the iteration behavior of ` Array ` object.
```
var arr = new Array();
console.log( typeof arr[Symbol.iterator] );  // function
```
Hint: We are using the ` Array ` as an object and the bracket notation to access a property in this object. Do not get confused with using bracket notation with Arrays to get elements inside.

#### Iterator Protocol
* **Iterator Protocol** defines a way to produce a sequence of values and a return value when all values have been produced.
* For an object to be **iterator** it must define a function with the name ` next `. This function returns a Generator object which we defined under **Generators** section.

#### Define your own Iterator
* Now let's see how the default **iterator** of ` Array ` works.
```
var friends = ["Mossab", "Ayman", "Ismail"];
var defaultIterator = friends[Symbol.iterator]();
console.log( defaultIterator.next() );  // {value: "Mossab", done: false}
console.log( defaultIterator.next() );  // {value: "Ayman", done: false}
console.log( defaultIterator.next() );  // {value: "Ismail", done: false}
console.log( defaultIterator.next() );  // {value: undefined, done: true}
```          
* Now let's define our own **iterator**.
```
function generateIterator( arr ) {
    return {
        next: function() {
            if ( arr.length > 0 ) {
                return { value: `Hello ya ${arr.shift()} Basha`, done: false };
            } else {
                return { done: true }
            }
        }
    };
}
var ourOwnIterator = generateIterator( friends );
console.log( ourOwnIterator.next() );  // {value: "Hello ya Mossab Basha", done: false}
console.log( ourOwnIterator.next() );  // {value: "Hello ya Ayman Basha", done: false}
console.log( ourOwnIterator.next() );  // {value: "Hello ya Ismail Basha", done: false}
console.log( ourOwnIterator.next() );  // {value: undefined, done: true}
```
Here we defined a function ` generateIterator ` that returns an **iterator** object(an object contains implementation for ` next ` function). Then we invoked this function and stored the returned value which is an **iterator** object and used that object to iterate through the array.  
Recall that this is a **closure**. The **lexical environment**(variable ` arr ` which is whatever passed to the ` generateIterator ` function(in this case it is ` friends `)) is attached to the ` next ` function.


## Chapter quiz
1. If a value is provided to a function that has default arguments, what happens to the default arguments?  
they are overwritten.
2. What is the purpose of object literal enhancement?  
shorter syntax, less typing.
3. What does an arrow function point to?  
whatever the function returns.
4. Besides aesthetics, why might you want to use arrow functions in your code?   
to keep 'this' in scope?  
5. Where can you use destructuring assignment?  
functions  
objects  
arrays  
6. How do we hit pause in a generator?  
use the yield keyword.
7. Why are Symbols primarily used?  
to create unique ids.
8. What is the iterable protocol?  
a way to customize iteration behavior.

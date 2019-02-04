## ES6 class syntax
* **ECMAScript 2015** introduced **classes** to **JavaScript**.
* Declaring **classes** in JavaScript is a bit different.
    * We use the keyword ` constructor ` to define a **constructor** to the class.
    * We define a **function** without using the ` function ` keyword.
    * We do not define **variables** explicitly. Instead we access them using ` this ` keyword.
* Let's see some example:
```
class Student {
    constructor( name, age, year ) {
        this.name = name;
        this.age = age;
        this.year = year;
    }
    describe() {
        console.log( `I'm a student in the ${this.year} year named ${this.name} aged ${this.age}.` );
    }
}
var students = [new Student("Hossam", 22, "Forth"), new Student("Ayman", 21, "Forth"), new Student("Ibrahim", 20, "Second")];
for ( let student of students ) {
    student.describe();
}
```
Output:  
I'm Hossam. A student in the Forth year. I'm 22 years old.  
I'm Ayman. A student in the Forth year. I'm 21 years old.  
I'm Ibrahim. A student in the Second year. I'm 20 years old.  
* In JavaScript we can use a ` function ` or a ` var ` before we declare it. This is called **Hoisting**. However, this is not applicable to a ` classe `.  
    * **Functions**  
    ```
    function testHoisting() {  // test
        console.log( "test" )
    }
    testHoisting();
    ```
    ```
    testHoisting();  // test
    function testHoisting() {
        console.log( "test" )
    }
    ```
    * **Variables**  
    ```
    var x = 5;
    function testHoisting () {
        console.log( x );
    }
    testHoisting();  // 5
    ```  
    ```
    function testHoisting () {
        console.log( x );  // We uses x before declaring it
    }
    var x = 5;
    testHoisting();  // 5
    ```    
        * **Hint**: **Hoisting** is working by saving functions and variables in **memory** during **compiling** thus in **running time** we can use them before the line of declaration. However, for variables what saved in memory is only the declaration not the initialization.
        ```
        console.log(x)  // undefined
        var x = 5;
        ```
        In this script, yes we x is saved in memory so we can use it but its initialization is not. That is equivalent to that:
        ```
        var x;
        console.log( x );  // undefined
        x = 5;
        ```
    * **Classes**
    ```
    class TestHoisting {
        constructor( x ) {
            this.x = x;
        }
    }
    var v = new TestHoisting( 5 );  // Valid
    ```
    ```
    var v = new TestHoisting( 5 );  // Not valid: TestHoisting is not defined
    class TestHoisting {
        constructor( x ) {
            this.x = x;
        }
    }
    ```


## Class inheritance
* We know the ` inheritance ` concept. It is what allows us to **reuse** existing classes with its properties and methods **avoiding redeclaring** them.
* For example if we are building an educational system for schools, institutes, and universities. Certainly there are properties and methods that are shared among those different entity sets. Instead of **repeating** those shared things in all entity sets, We define them in a **super** class and **inherit** from that class our three classes: School, Institute, and University so that all what we defined in the **super** class is available in its **subclasses**.
* Let's see the **syntax**. Building upon the previous **Student** ` class `.
```
class SubStudent extends Student {
    constructor( name, age, year ) {
        super(name, age, year);
    }
}
var x = new Substudent("Mr.x", 19, "First");
x.describe();  // I'm a student in the First year named Mr.x aged 19.
```
The **Substudent** ` class ` **inherited** the ` function ` **describe** from the **Student** ` class `.


## Getters and setters
* We are familiar with **getter** and **setter** functions. They are used to provide a predefined way to access properties which ensures **encapsulation**.
* **getter** and **setter** functions are used in a bit different way in JavaScript.
    * We do not use ` function ` keyword to **declare** them. Instead we use ` get ` keyword for **getter** and ` set ` keyword for **setters.
    * We do not use ` function ` syntax to **call** them. Instead we treat them as properties using dot notation.
* Let's see an example:
```
class Student {
    constructor( _name, _age ) {
        this._name = _name;
        this._age = _age;
        this._friends = [];
    }
    get name() {
        return `student name: ${this._name}`;
    }
    set name( _name ) {
        this._name = _name;
    }
    get age() {
        return `student age: ${this._age}`;
    }
    set age( _age ) {
        this._age = _age;
    }
    get friends() {
        return this._friends.join(', ');
    }
    set addFriend( friend ) {
        this._friends.push( friend );
    }
}
```
Now let's use these **getters** and **setters**.
```
var s = new Student( "Hossam", 22 );
s.addFriend = "Ayman";
s.addFriend = "Mossab";
s.addFriend = "Ismail";
console.log( s._name );  // Hossam
console.log( s.name );  // student name: Hossam
console.log( s._age );  // 22
console.log( s.age );  // student age: 22
console.log( s._friends );  // ["Ayman", "Mossab", "Ismail"]
console.log( s.friends );  // Ayman, Mossab, Ismail
```


## Chapter quiz
1. What happens when you use the 'new' keyword with ES6 classes?  
a new instance of the class is created.
2. When a new instance of a class is created, what happens to the custom methods in the base class?  
They are inherited by the subclass.

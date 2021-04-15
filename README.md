# JS
## OOP
### Object
ECMAScript has no concepts of classes. Object is defined as "unordered collection of properties each of which contains a primitive value, object, or function". This means that an object is an array of values in no particular order.

###### Create a custom object with Object constructor
```javascript
// Create a new instance of Object and add properties and method to it
var person = new Object();
person.name = 'Jun';
person.age = 26;
person.job = 'FED Dev';
person.sayName = function() {
  console.log(this.name);
};
```

###### Create a custom object with object literals
```javascript
var person = {
  name: 'Jun',
  age: 26,
  job: 'FED Dev',
  sayName: function() {
    console.log(this.name);
  }
};
```

### The Factory Pattern
The factory pattern is well-known design to abstract away the process of creating specific objects. With no way to create classes in ECMAScript, developers created functions to encapsulate the creation of objects with specific interfaces.
```javascript
function createPerson(name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    console.log(this.name);
  };
  return o;
}

var person1 = createPerson('Junming', 26, 'FED Dev');
var person2 = createPerson('Bob', 24, 'BED Dev');
```
This pattern doesn't address the issue of object identification (what type of object an object is).

### The Constructor Pattern
Constructor in ECMAScript are used to create specific types of object.

###### Define custom constructor that define properties and methods for your own type of object

```javascript
// ConstructorPatternExample01

// By convention, constructor function always begin with an uppercase letter
// Constructors are simply functions that create objects
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() {
    console.log(this.name);
  };
}

var person1 = new Person('Junming', 26, 'FED Dev');
var person2 = new Person('Bob', 24, 'BED Dev');

// Calling a constructor with `new` keyword causes 4 steps to be taken:
// 1. Create a new object
// 2. Assign the `this` value of the constructor to the new object
//    (so `this` points to the new object)
// 3. Excute the code inside the constructor
//    (adds properties to the new object)
// 4. Return the new object
```

###### Compare to the factory pattern
1. There is no object being created explicitly
2. The properties and method are assigned directly onto the `this` object
3. There is no `return` statement

###### Constructors as Functions
Any function that is called with the new operator acts as a constructor

```javascript
// use as a constructor
var person = new Person('Junming', 26, 'FED Dev');
person.sayName(); // 'Junming'

// call as function
Person('Bob', 24, 'BED Dev');
window.sayName(); // 'Bob' --> properties and methods get added to the `window` object

// call in the scope of another object
var o = new Object();
Person.call(o, 'Kristen', 25, 'Designer');
o.sayName(); // 'Kristen'
```

###### Problems with Constructors
The major downside to constructors is that methods are created once for each instance.
In ConstructorPatternExample01, both person1 and person2 have a method call sayName(), 
but those methods are not the same instance of Function. We can view the constructor as:
```javascript
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = new Function("console.log(this.name)");
}

console.log(person1.sayName == person2.sayName); // false
```

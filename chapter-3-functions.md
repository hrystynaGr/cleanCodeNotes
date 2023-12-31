## Chapter 3: Functions

In the opening part of the chapter, the author introduces us to function names, function arguments, and guidelines concerning how long functions should be and their level of abstraction. Let's delve into these ideas with a few JavaScript examples.

In the latter part of the chapter, the author acquaints us with error handling, try/catch blocks, and the DRY (Don't Repeat Yourself) principle. Furthermore, the author sheds light on the idea that functions should not both modify and compare things, but rather either modify or compare them. Additionally, the "no side effect" principle is highlighted.

**Function Length**
Back when FORTRAN was very popular, it was recommended that functions should be "no longer than the screen height." However, it's important to note that, at that time, the available screen space could only accommodate about 24 lines of code. This rule made sense then, but in today's context, where we can fit over 150 lines on one screen, we should not adhere to the "rule of a screen" anymore. The author summarizes it as "the smaller, the better." Functions should be as concise as possible without sacrificing clarity.

**Functions Should Do One Thing**
Avoid having your function perform multiple tasks. Each function should have a single, well-defined purpose.

**Functions Should Maintain a Single Level of Abstraction**
Functions can have three levels of abstraction:
1. High-level: This level deals with the overall structure of the code, architecture, system components, and their interactions.
2. Mid-level: It focuses on specific modules, components, or classes and how they behave.
3. Low-level: Involves granular code constructs like loops, calculations, and conditional statements.

It's essential not to mix these abstraction levels in a single function. For instance, it's not good practice for a function to calculate the sum of two values (low-level) and create a new class (mid-level) simultaneously.

**Function Names**
Ideally, function names should use a verb as the function name and a noun as an argument, such as `write(name)` or `get(guests)`. If a function cannot be adequately described with a single word, use two, three, or even four words. A longer but descriptive name is preferable to a short, nondescriptive one.

**Arguments**
Functions with more than two arguments are challenging to work with. They are difficult to read, understand, write, and test. Niladic functions (functions with no arguments) are considered ideal. Monadic functions (functions with one argument) have several appropriate use cases, such as when the argument is an event, when you modify the argument and return it, or when you need to ask a question about the argument. Dyadic functions (functions with two arguments) can sometimes be simplified into monadic functions, such as `outputStream.writeField(name)`.

Functions with boolean (flag) arguments should be avoided as they indicate a function's complexity. If a function requires three arguments (triadic functions), you should carefully consider whether there is a more elegant solution. Consider using argument objects when it makes sense to group function arguments logically into a single object.

**Switch Statement**
The switch statement should only exist in the basement of an abstract factory, hidden from view. The factory can use the switch statement to create instances of derived classes of `Employee`, and functions like `calculatePay`, `isPayday`, and `deliverPay` can be dispatched polymorphically through the Employee interface.

Let's explore some JavaScript examples. First, let's refresh our memory on how polymorphism works in JavaScript.

**Polymorphism** is a fundamental concept in programming and object-oriented design that enables objects of different classes to be treated as instances of a common superclass. In simpler terms, it allows different classes or data types to respond to the same method or interface. There are two types of polymorphism.
1. **Compile-time (Static) Polymorphism** occurs when you have multiple methods in the same class with the same name but different parameters. The appropriate method to execute is determined at compile time based on the method signature. While not as common in JavaScript as in statically typed languages, you can achieve some form of static polymorphism in JavaScript by using a different number of arguments.

```
function add(a, b) {
  return a + b;
}

function add(a, b, c) {
  return a + b + c;
}

console.log(add(2, 3));       // This will output 5
console.log(add(2, 3, 4));    // This will output 9

```
2. `Runtime (Dynamic) Polymorphism` -  It occurs when you have a hierarchy of classes with a parent class and one or more child classes. The child classes provide their own implementation of a method that is already defined in the parent class.
```
class Shape {
  area() {
    return 0;
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }

  area() {
    return Math.PI * this.radius * this.radius;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  area() {
    return this.width * this.height;
  }
}

// Polymorphism in action
const shapes = [new Circle(5), new Rectangle(4, 6)];

shapes.forEach(shape => {
  console.log(`Area: ${shape.area()}`);
});
```
In this example, we have a `Shape` class with an `area` method. Both `Circle` and `Rectangle` classes inherit from `Shape` and provide their own implementations of the `area` method. The `shapes` array contains objects of both `Circle` and `Rectangle` types. When we call `shape.area()`, JavaScript uses runtime polymorphism to invoke the appropriate area method based on the actual object type, which leads to the correct area calculation.

> "Step Down Rule" - you should be able to read your code as a book, starting from the hight level functions followed by more detailed functions and then low level ones. Look at it as at "to" concept in english.

```
To cook the cake I should put cream between the dought.
    To cook the dough I should mix eggs, flour, and melted butter.
    To make the cream I should mix whipped cream and sugar.
        To melt the butter I should heat it.
        To get a whipped cream I shluld whipp it.
            Mix eggs, flour, melted butter.
            Mix whipped cream, sugar.
```

**Your function should have no side effects**
It means, that it should do strictly what it says it does. For example the following function clains that it adds an item to the shopping list.
```
function addToShoppingList(item) {
  shoppingList.push(item);
  shoppingList.sort((a,b) => a-b);
  return shoppingList;
}
```
But it also sorts the list, which may not alway wanted why you use function that clains that it only adds.

**Command Query Separation**
Functions should either do something or answer something, but not both. Either your function should change the state of an object, or it should return some information about that object. Doing both often leads to confusion. Consider, for example, the following function:
```
function setName(person, name) {
  if(typeof person === object && !person.hasOwnProperty(name)){
    setName(person, 'Bob')
    return true;
  }
  else {
    return false ;
  }
}
```
And imagine this function returns `true` if setting was sucessfull and `false` if it was not. This is how the usage of the functing will look like:
```
if(setName(person, 'Bob'))
```
Does it look clear? Not for me. What is the condition? If name already exist in person, or f we are allowed to chnage it or what?
It is much better to write it like that:
```
function setName(person, name) {
    setName(person, 'Bob')
}

if(typeof person === object && !person.hasOwnProperty(name)){
  setName(person, 'Bob')
}
```

**Extract Try/Catch Blocks (personally, do not agree with this)** 
Author suggests that insted of this function:
```
function deletePageAndAllReferences(page) {
  try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
  } catch (e) {
    logError(e);
  }
}
```
we should create two functions, one of which handles `try/catch` login, and second contains the logic from `try`.
```
function deletePageAndAllReferences(page) {
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
}

function delete() {
  try {
    deletePageAndAllReferences(page)
  } 
  catch (e) {
    logError(e);
  }
}
```

**Why I do not fully like this idea?**
Let's envision a scenario where we need to remove 20 distinct instances. In this case, what should we name these 20 different `delete` functions? You will need to provide 20 unique names for functions that carry out the actual deletion, as well as 20 names for functions responsible for handling the deletion logic. Does't sound fun right.
However we can suppose that author was suggesting more concise approach - creating a single `delete` function and passing another function as an argument (callback), specifying what should be deleted. However, is dealing with functions as arguments and the concept of callback functions any more convenient to write and read compared to adding just two more lines in a catch block? In my opinion, not necessarily.


**Don’t Repeat Yourself (DRY)**

Duplication may be the root of all evil in software. Many principles and practices have been created for the purpose of controlling or eliminating it. Just do not do it. If you need to duplicate the code it means that you need to ceate another structure to omit it. Use function/mathod/class/component what is better suited in your case to omit duplication.
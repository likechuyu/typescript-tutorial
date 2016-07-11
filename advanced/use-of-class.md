# 类的用法

传统方法中，JavaScript 通过构造函数实现类的概念，通过原型链实现继承。而在 ES6 中，我们终于迎来了 `class`。

TypeScript 除了实现了所有 ES6 中的类的功能以外，还添加了一些新的用法。

这一节主要介绍类的用法，下一节再介绍如何定义类的类型。

## 类的概念

虽然 JavaScript 中有类的概念，但是可能大多数 JavaScript 程序员并不是非常熟悉类，这里对类相关的概念做一个简单的介绍。

- 类(Class)：定义了一件事物的抽象特点，包含它的属性和方法
- 对象（Object）：类的实例，通过 `new` 生成
- 面向对象（OOP）的三大特性：封装、继承、多态
- 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据。比如一个账单对象，拥有打印账单方法，外界可以直接调用这个方法即可打印账单，同时外界无法直接更改账单内容
- 继承（Inheritance）：子类继承父类，即子类除了拥有父类的所有特性外，还有一些更具体的特性
- 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法，此时针对一个对象实例，我们无需了解它是 `Cat` 还是 `Dog`，直接调用 `eat` 方法也是可以的，程序会自动判断出来应该执行哪个 `eat`
- 存取器（getter & setter）：用以改变属性的赋值和读取行为
- 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public` 表示是公有属性或方法
- 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
- 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

## ES6 中类的用法

我们先回顾一下 ES6 中类的用法，更详细的介绍可以参考 [ECMAScript 6 入门 - Class]。

### 属性和方法

使用 `class` 定义类，使用 `constructor` 定义构造函数，通过 `new` 生成新实例的时候，会自动调用构造函数。

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

let a = new Point(1, 2);
console.log(a.toString()); // (1, 2)
```

### 类的继承

使用 `extends` 关键字实现继承，子类中使用 `super` 表示父类的构造函数。

```js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}

let b = new ColorPoint(1, 2, 'red');
console.log(b.toString()); // red (1, 2)
```

### 存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为：

```js
class Point {
  constructor() {
    // ...
  }
  get x() {
    return 1;
  }
  set x(value) {
    console.log('setter: ' + value);
  }
}

let a = new Point();

a.x = 2; // setter: 2
console.log(a.x); // 1
```

### 静态方法

使用 `static` 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用：

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod(); // 'hello'

var foo = new Foo();
foo.classMethod(); // TypeError: undefined is not a function
```

## ES7 中类的用法

ES7 中有一些类的提案，TypeScript 也实现了它们，这里做一个简单的介绍。

### 实例属性

ES6 中实例的属性只能通过构造函数中的 `this.xxx` 来定义，ES7 提案中可以直接在类里面定义：

```js
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp);
  }
}

let a = new MyClass(); // 42
```

### 静态属性

ES7 提案中，可以使用 `static` 定义一个静态属性：

```js
class MyClass {
  static myStaticProp = 42;

  constructor() {
    // ...
  }
}

console.log(MyClass.myProp); // 42
```

## TypeScript 中类的用法

### public private 和 protected

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 `public`、`private` 和 `protected`。

`public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的。

`private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问。

`protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是她在子类中也是允许被访问的。

下面举一些例子：

```ts
class Animal {
  public name;
  public constructor(theName) { this.name = theName; }
}

let cat = new Animal('Tom');
console.log(cat.name); // Tom
cat.name = 'Jack';
console.log(cat.name); // Jack
```

上面的例子中，`name` 被设置为了 `public`，所以直接访问实例的 `name` 属性是允许的。

很多时候，我们希望有的属性是无法直接存取的，这时候就可以用 `private` 了：

```ts
class Animal {
  private name;
  public constructor(theName) { this.name = theName; }
}

let cat = new Animal('Tom');
console.log(cat.name); // Tom
cat.name = 'Jack';

// index.ts(7,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
// index.ts(8,1): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

需要注意的是，TypeScript 编译之后的代码中，并没有限制 `private` 属性在外部的可访问性。

上面的例子编译后的代码是：

```js
var Animal = (function () {
    function Animal(theName) {
        this.name = theName;
    }
    return Animal;
}());
var cat = new Animal('Tom');
console.log(cat.name);
cat.name = 'Jack';
```

使用 `private` 修饰的属性或方法，在子类中也是不允许访问的：

```ts
class Animal {
  private name;
  public constructor(theName) { this.name = theName; }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}

// index.ts(9,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

而如果是用 `protected` 修饰，则允许在子类中访问：

```ts
class Animal {
  protected name;
  public constructor(theName) { this.name = theName; }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}
```

### 抽象类

`abstract` 用于定义抽象类和抽象类中的抽象方法。

```ts
abstract class Animal {
  public name;
  public constructor(theName) { this.name = theName; }
  public abstract move();
}

let cat = new Animal('Tom');

// index.ts(7,11): error TS2511: Cannot create an instance of the abstract class 'Animal'.
```

上面的例子中，我们定义了一个抽象类 `Animal`，并且定义了一个抽象方法 `move`。在实例化抽象类的时候报错了。

抽象类是不允许被实例化的。

```ts
abstract class Animal {
  public name;
  public constructor(theName) { this.name = theName; }
  public abstract move();
}

class Cat extends Animal {
  public eat() {
    console.log(`${this.name} is eating.`);
  }
}

let cat = new Cat('Tom');

// index.ts(7,7): error TS2515: Non-abstract class 'Cat' does not implement inherited abstract member 'move' from class 'Animal'.
```

上面的例子中，我们定义了一个抽象类 `Cat` 继承了 `Animal`，但是没有实现抽象方法 `move`，所以编译报错了。

抽象方法必须被实现。

```ts
abstract class Animal {
  public name;
  public constructor(theName) { this.name = theName; }
  public abstract move();
}

class Cat extends Animal {
  public move() {
    console.log(`${this.name} is moving.`);
  }
}

let cat = new Cat('Tom');
```

上面的例子中，我们实现了抽象方法 `move`，编译通过了。

需要注意的是，即使是抽象方法，TypeScript 的编译结果中，任然会存在这个类，上面的代码的编译结果是：

```js
var __extends = (this && this.__extends) || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
};
var Animal = (function () {
    function Animal(theName) {
        this.name = theName;
    }
    return Animal;
}());
var Cat = (function (_super) {
    __extends(Cat, _super);
    function Cat() {
        _super.apply(this, arguments);
    }
    Cat.prototype.move = function () {
        console.log(this.name + " is moving.");
    };
    return Cat;
}(Animal));
var cat = new Cat('Tom');
```

### 实现接口

接口在 TypeScript 中是一个非常灵活的概念，实际上，我们在[对象的类型——接口](/from-javascript-to-typescript/basics/type-of-object-interfaces.html)一章中，已经接触了接口的一种用法了。这里是接口的第二种用法。想了解接口的所有用法，可以参考？？？。

实现（implements）是面向对象中的一个重要概念。一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 `implements` 关键字来实现。这个特性大大提高了面向对象的灵活性。

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它：

```ts
interface Alarm {
  alert()
}

class Door {
}

class SecurityDoor extends Door implements Alarm {
  alert() {
    console.log('SecurityDoor alert');
  }
}

class Car implements Alarm {
  alert() {
    console.log('Car alert');
  }
}
```

## Links

- [Handbook - Classes](http://www.typescriptlang.org/docs/handbook/classes.html)
- [中文手册 - 类](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Classes.html)
- [ECMAScript 6 入门 - Class]

[ECMAScript 6 入门 - Class]: http://es6.ruanyifeng.com/#docs/class
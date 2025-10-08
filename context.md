# Задачи для практики к собеседованиям на frontend-разработчика - Контекст

## Контекст

### ✅ Задача

Меняем прототип. Что будет выведено в консоль?

```ts
function Person(name) {
    this.name = name;
}

const juan = new Person("Juan");

Person.prototype = {
  getName: function () {
		return this.name;
	},
};

const pedro = new Person("Pedro");

console.log(pedro.getName());
console.log(juan.getName());
```

<details>
  <summary>Решение</summary>

Происходит полная перезапись прототипа класса, но контруктор this.name остается на месте.
Надо поминть, что class это синатксический сахар над функциями и эти записи идентичны 


```ts
function Person(name) {
    this.name = name;
}
// РАВНО
class Person {
	construstor(name) {
		this.name = name;
	}
}

console.log(pedro.getName()); // pedro создан от нового прототипа, у которого уже присутствует метод getName - будет выведено поле name
console.log(juan.getName()); // juan создан от старого прототипа, у которого не было конструктора, поэтому он не имеет метода getName - будет ошбика

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### ✅ Задача

Что будет выведено и как это исправить?

```ts

let obj = {
    name: 'David',
    getName() {
        console.log(`name is: ${this.name}`);
    },
};

let fn = obj.getName;

fn();
```

<details>
  <summary>Решение</summary>


```ts
let obj = {
    name: 'David',
    getName() {
        console.log(`name is: ${this.name}`);
    },
};

let fn = obj.getName.bind(obj);

fn();
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 
 ### ✅ Задача

Что будет выведено в консоль?

```ts
let obj1 = {
    name: 'User 1',
    getName() {
        console.log(`name is: ${this.name}`);
    },
};

let obj2 = {
    name: 'User 2',
    getName() {
        console.log(`name is: ${this.name}`);
    },
};

let fn = obj.getName.bind(obj2).bind(obj1);

fn();
```

<details>
  <summary>Решение</summary>

  Функцию bind можно выполнить только 1 раз. Это нужно запомнить как факт. [Объяснение](https://dev.to/akashkava/functionbindbind-does-not-work-in-javascript-59am)
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 
### Задача

 Что будет выведено в консоль?

```ts
const person = { name: "Vasya", age: 22 };
const position = { title: "Software Engineer" };

person.position = position;
person.postion.salary = 120;

console.log(person.position);
console.log(position);
```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### ✅ Задача

```ts
function foo() {
  const x = 10;
  return {
    x: 20,
    bar: function () {
      console.log(this.x);
    },
    baz: () => {
      console.log(this.x);
    },
  };
}

const obj = foo();
obj.bar(); // ? 
obj.baz(); // ? 

const obj2 = foo.call({ x: 30 });
let y = obj2.bar;
let x = obj2.baz;

y(); // ? 
x(); // ? 

obj2.bar(); // ? 
obj2.baz(); // ?
```

<details>
 <summary>Ответ</summary>

```ts
const obj = foo();
obj.bar(); // 20
obj.baz(); // undefined

const obj2 = foo.call({ x: 30 });
let y = obj2.bar;
let x = obj2.baz;

y(); // undefined
x(); // 30

obj2.bar(); // 20
obj2.baz(); // 30
```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### ✅ Задача

Что будет выведено в консоль?

В чем разница hasOwnProperty и in?  
Что будет выведено в консоль? 

```ts
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  sound() {
    console.log("Some sound");
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  
  bark() {
    console.log("Woof woof!");
  }
}

let myDog = new Dog("Buddy", "Labrador");

console.log(myDog.hasOwnProperty('name')); // ?
console.log(myDog.hasOwnProperty('sound')); // ?

console.log('name' in myDog); // ?
console.log('sound' in myDog); // ?
```

<details>
  <summary>Решение</summary>

```ts
console.log(myDog.hasOwnProperty('name')); // true
console.log(myDog.hasOwnProperty('sound')); // false

console.log('name' in myDog); // true
console.log('sound' in myDog); // true
```
  
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Что будет выведено в консоль?

```ts
class Fruit {}

Object.assign(Fruit.prototype, {
    color: 'red',
    names: [],
    addName (name) {
        this.names.push(name)
        
    }
})

const fruit1 = new Fruit();
const fruit2 = new Fruit();

fruit1.color = 'yellow';

fruit1.addName('banana')
fruit1.addName('coconut')

console.log('20:', fruit1.color, fruit2.color)
console.log('21:', fruit1.names, fruit2.names)

delete fruit2.names;

console.log('24:', fruit2.names, Fruit.prototype.names)

fruit2.names = []
fruit2.addName('apple')

console.log('29:', fruitl.names, fruit2.names)

console.log('31:', Fruit.prototype)
console.log('32:', Fruit.prototype.constructor)
console.log('33:', Fruit.prototype.constructor.prototype)
console.log('34:', Fruit.prototype.constructor.prototype.prototype)
```

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
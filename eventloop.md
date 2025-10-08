# Задачи для практики к собеседованиям на frontend-разработчика - Event Loop

## Event Loop

### ✅ Задача

Что будет выведено в консоль?

```ts 
setTimeout(() => console.log('setTimeout 1'), 0);

new Promise((resolve, reject) => {
    console.log('Promise 1');
    resolve();
    console.log('Promise 2');
}).then(() => console.log('Promise 3'));

Promise.resolve().then(() => setTimeout(() => console.log('setTimeout 2'), 0));

Promise.resolve().then(() => console.log('Promise 4'));

setTimeout(() => console.log('setTimeout 3'), 0);

console.log('final');
```

<details>
  <summary>Решение</summary>

```ts
Promise 1

Promise 2

final

Promise 3

Promise 4

setTimeout 1

setTimeout 3

setTimeout 2
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### ✅ Задача
Логика решения в прошлой задаче

```ts
setTimeout(() => console.log(1));

new Promise(function (resolve, reject) {
    resolve();
  })
  .then(() => console.log(2))
  .then(() => console.log(3))
  .catch(() => console.log("err"))
  .then(() => setTimeout(() => console.log(4)));
  
console.log(5);

```

<details>
  <summary>Решение</summary>

```ts
5
2
3
1
4
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

```ts
// Задача 1
Promise.reject("a")
.catch((p)=> p + "b")
.catch((p)=> p + "c")
.then((p)=>p + "d")
.then((p)=>console.log(p))

// Задача 2
Promise.reject("a")
    .then((p)=>p + "2")
    .then((p)=>p + "3")
    .then((p)=> {
        throw new Error('4')
    })
    .catch((p)=> p + "5")
    .catch((p)=> p + "7")
    .then((p)=>p + "8")
    .catch((p)=> p + "9")
```

<details>
  <summary>Решение</summary>

Первый catch обрабатывает первую ошибку, выборшенную через reject.  
Дальше в catch не проваливаемся и можно обрабатывать полученное значение

```ts

// Задача 1
Promise.reject("a")
.catch((p)=> p + "b") // отловили reject
.catch((p)=> p + "c") // пропускаем, так как уже отловили ошибку и получили результат
.then((p)=>p + "d") // обработали результат
.then((p)=>console.log(p)) // обработали результат

// Результат abd


// Задача 2
Promise.reject("a")
.then((p)=>p + "2") // пропускаем, так как нужно отловить ошибку
.then((p)=>p + "3") // пропускаем, так как нужно отловить ошибку
.then((p)=> { throw new Error('4') }) // отловили reject. И отдали еще одну ошибку
.catch((p)=> p + "5") // обрабатываем выброшенную ошибку
.catch((p)=> p + "7") // пропускаем, так как уже отловили ошибку и получили результат
.then((p)=>p + "8") // обработали результат
.catch((p)=> p + "9")  // пропускаем, так как уже отловили ошибку и получили результат

// Результат a58
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

```ts
console.log('A')

const p = new Promise((resolve) => {
  resolve('');
  console.log('B')
});

p.then(() => {
  p.then(() => console.log('C'));
  console.log('D');
});

setTimeout(() => {
   console.log('E')
}, 0)

p.then(() => console.log('F'));

```

<details>
    <summary>Решение</summary>

```ts
A
B
D
F
C
E
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Что будет выведено в консоль?

```ts
setTimeout(function timeout() {
    console.log(1);
}, 0);

let p = new Promise(function(resolve, reject) {
    console.log(2);
    resolve();
});

p
.then(function() {
    console.log(5);
}).then(function() {
    console.log(6);
});

p
.then(function() {
    console.log(7);
}).then(function() {
    console.log(8);
});

console.log(4);

```

<details>
    <summary>Решение</summary>

```ts

2 4 5 7 6 8 1
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### Задача

Что будет выведено в консоль?

```ts

async function f() {
    console.log(1);
    
    const promise = new Promise((resolve) => {
        console.log(2);
        setTimeout(() => {
            console.log(3);
            resolve('готово!');
            console.log(4);
        });
    });
    
    console.log(5);

    const result = await promise;
    console.log(6);
    console.log(result);
    
    return 'Result';
}

f();
console.log(7);


```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

Что будет выведено и почему?

```ts
var p1 = new Promise(function(resolve, reject) {
    console.log(5);
    setTimeout(resolve, 500, "1");
});
var p2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, "2");
});

Promise.race([p1, p2]).then(function(value) {
  console.log(value);
});
```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Что будет выведено в консоль?

```ts
console.log('77')

const p = new Promise((_, reject) => {
    setTimeout(() => {
        console.log('reject');
        reject();
    }, 2000);
});

p
.then(
    () => console.log('10'),
    () => console.log('11')
)
.then(
    () => console.log('13'),
    () => console.log('14')
);

p
.then(
    () => console.log('18'),
    () => console.log('19')
);

p.then(
    () => console.log('23'),
    () => console.log('24')
);


```

<details>
    <summary>Решение</summary>

```ts
77
reject
11
19
24
13

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Что будет выведено в консоль?

```ts
setTimeout(() => {
    console.log('setTimeout 100');
    new Promise(resolve => {
            setTimeout(resolve, 1000)
        })
        .then(() => {
            console.log('sleep 1000 then');
        });
}, 100);

const promise = new Promise(resolve => {
    console.log('in promise');
    resolve('Promise then');
});

new Promise(resolve => {
        setTimeout(resolve, 500)
    })
    .then(() => {
        console.log('sleep 2000 then');
    })
    .finally(() => {
        console.log('sleep 2000 finally');
        setTimeout(() => {
            console.log('finally setTimeout 1000');
        }, 1000);
    });

console.log('log1');
promise.then(res => console.log(res));
```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Что будет выведено в консоль?

```js
const a = 2;
const b = 3;
const c = 5;

const promise = new Promise(res => res(a));

setTimeout(() => {
    console.log(b);
}, 0);

promise
    .then(v => {
        console.log(v);
        return v * a;
    })
    .catch(() => {
        console.log(v);
    })
    .finally(v => {
        console.log(v);
        return v * a;
    })
    .then(v => {
        console.log(v);
        return v * a;
    });

console.log(c);
```

<details>
    <summary>Решение</summary>

```js
5
2
undefined
4
3

```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Что будет выведено в консоль?


```js
const myPromise = () => Promise.resolve("I have resolved!");

function firstFunction() {
    myPromise().then(res => console.log(res));
    console.log('first');
}

async function secondFunction() {
    console.log(await myPromise());
    console.log('second');
}

firstFunction();
secondFunction();
```

<details>
    <summary>Решение</summary>

```js
first
I have resolved!
I have resolved!
second
```
</details>
# Задачи для практики к собеседованиям на frontend-разработчика - задачи, приближенные к реальной жизни

## Задачи из реальной жизни

### ✅ Задача

Нужно написать чистый читаемый код
Рассчитать оплату за проживание в отеле с учетом рабочих и праздничных дней
 
Функция может принимать 2 аргумента  
1. Количество ночей проживания в отеле (обязательный параметр)  
2. Дата заселения (необязательный параметр). Если значение не указано, то отсчет ведется от текущего дня  
- Стоимость проживания в будние дни (с понедельника по пятницу) стоит 1500 руб  
- Стоимость проживания в выходные дни (суббота, воскресенье) стоит 2200 руб  

```ts
const prices = {
  weekday: 1500,
  holiday: 2200,
};

function bookingCalculate() {
  
}

console.log(bookingCalculate(7)); // 11900
console.log(bookingCalculate(3, new Date('2023-11-10'))); // 5900
```

<details>
  <summary>Решение</summary>

```ts
const prices = {
  weekday: 1500,
  holiday: 2200,
};

const addDays = (date, days) => {
  const result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
};

const checkIsHoliday = day => {
  return !checkIsWeekdaty(day);
};

const checkIsWeekdaty = date => {
  const day = new Date(date).getDay();
  return day > 0 && day < 6;
};

function bookingCalculate(nights, startDate = new Date()) {
  const endDate = addDays(startDate, nights);

  let tempStartDate = startDate;
  let weekDays = 0;
  let hoilDays = 0;

  while (tempStartDate < endDate) {
    if (checkIsWeekdaty(tempStartDate)) {
      weekDays += 1;
    }
    if (checkIsHoliday(tempStartDate)) {
      hoilDays += 1;
    }
    tempStartDate = addDays(tempStartDate, 1);
  }
  return weekDays * prices.weekday + hoilDays * prices.holiday;
}

console.log(bookingCalculate(7)); // 11900
console.log(bookingCalculate(3, new Date('2023-11-10'))); // 5900

```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->




### ✅ Задача

Реализовать и типизировать собственный pick

```ts
const pick = (obj, ...keys) => {

}
```

<details>
  <summary>Решение</summary>

```ts
const pick = (obj, ...keys) => {
  return keys.reduce((acc, key) => {
    acc[key] = obj[key];
    return acc;
  }, {});
};

console.log(pick({ a: 1, b: 2, c: 3 }, 'a', 'c'));


type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### ✅ Задача

1) Написать аналог функции map
```ts
const customMap = () => {}

const array = [{ id: 1 }, { id: 2 }];
const format = (el, index) => `${el.id}|${index}`;
const formattedArray = customMap(array, format);

```

2) Написать полифил для map, используя метод из предыдущего пункта (обращаться к Array.prototype)
```ts
const format = (el, index) => `${el.id}|${index}`;
const array = [{ id: 1 }, { id: 2 }];

const formattedArray = array.customMap(format);

```

<details>
  <summary>Решение</summary>

1) Написать аналог функции map

```ts
const customMap = (array, callback) => {
  const result = [];

  for (let i = 0; i < array.length; i++) {
    result.push(callback(array[i], i));
  }

  return result;
};
```

2) Написать полифил для map, используя метод из предыдущего пункта (обращаться к Array.prototype)

```ts
Array.prototype.customMap = function(callback) {
  const result = [];

  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i));
  }

  return result;
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 
### ✅ Задача

1. Вернуть объект с ключами type из масива, значение - массив элементов с таким type  
2. Вернуть объект с ключами type, а значение - объект вида {count: количество, weight: суммарный вес}  

```ts
const arr = [
  { type: "banana", weight: 32 },
  { type: "apple", weight: 24 },
  { type: "kiwi", weight: 55 },
  { type: "banana", weight: 44 },
  { type: "orange", weight: 5 },
];

const groupByType = (arr) => {
  
};

console.log(groupByType(arr))
```

<details>
  <summary>Решение</summary>

1. Вернуть объект с ключами type из масива, значение - массив элементов с таким type  

```ts
const groupByType = (arr) => {
  const result = {};

  arr.forEach((item) => {
    const { type, ...rest } = item;

    if (result[type]) {
      result[type].push(rest);
    } else {
      result[type] = [rest];
    }
  });

  return result;
};

```

2. Вернуть объект с ключами type, а значение - объект вида {count: количество, weight: суммарный вес}  

```ts
const groupByType = (arr) => {
  const result = {};

  arr.forEach((item) => {
    const { type, weight } = item;

    if (result[type]) {
      result[type].count++;
      result[type].weight += weight;
    } else {
      result[type] = { count: 1, weight };
    }
  });

  return result;
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 
### ✅ Задача

Имеется исходный массив плоских данных
  - Необходимо преобразовать его в структуру, где данные будут сгруппированы по одному из полей (кроме id)
  - Внутри сформированной группы должен лежать объект, ключами в которого должно стать поле (к примеру id)
  - Значением должен быть объект из исходного массива с соответствующем полем id, не включая само поле id
```ts
  const data = [
    { id: 1, age: 20, name: 'Иван', country: 'Russia' },
    { id: 2, age: 20, name: 'Дмитрий', country: 'USA' },
    { id: 3, age: 20, name: 'Алексей', country: 'Russia' },
    { id: 4, age: 20, name: 'Александр', country: 'USA' },
    { id: 5, age: 20, name: 'Иван', country: 'Russia' },
  ];

const groupBy = () => {

}

console.log(groupBy(data, 'country', 'id'));

  
// Должно получиться
  const result = {
    Russia: {
      1: { age: 20, name: 'Иван', country: 'Russia' },
      3: { age: 20, name: 'Алексей', country: 'Russia' },
      5: { age: 20, name: 'Иван', country: 'Russia' },
    },
    USA: {
      2: {age: 20, name: 'Дмитрий', country: 'USA' },
      4: { age: 20, name: 'Александр', country: 'USA' },
    }
  }
```

<details>
  <summary>Решение</summary>

```ts
  
const groupBy = (data, key, innerKey) => {
  return data.reduce((acc, current) => {
    const fieldByKey = current[key];
    console.log(current);

    if (!acc[fieldByKey]) {
      acc[fieldByKey] = {};
    }

    const innerKeyValue = current[innerKey];

    const rest = omit(current, 'id', 'age');

    acc[fieldByKey][innerKeyValue] = rest;

    return acc;
  }, {});
};

const omit = (obj, ...keys) => {
  const objectOfKeys = keys.reduce((acc, current) => {
    acc[current] = true;
    return acc;
  }, {});

  return Object.keys(obj).reduce((acc, key) => {
    if (objectOfKeys[key]) {
      acc[key] = obj[key];
    }

    return acc;
  }, {});
};

console.log(groupBy(data, 'country', 'id'));

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

```js
// Задача: получить таблицу по ролям + totalWeight:
// {
//   'сталь': { ids: [ 'ab', 'cd', 'fg' ], totalWeight: 8 },
//   'чугун': { ids: [ 'bc', 'de', 'ef' ], totalWeight: 11 },
// }


const goods = [
    { id: 'ab', name: 'Имя-01', type: 'сталь', weight: 1 },
    { id: 'bc', name: 'Имя-02', type: 'чугун', weight: 2 },
    { id: 'cd', name: 'Имя-03', type: 'сталь', weight: 3 },
    { id: 'de', name: 'Имя-04', type: 'чугун', weight: 4 },
    { id: 'ef', name: 'Имя-05', type: 'чугун', weight: 5 },
    { id: 'fg', name: 'Имя-06', type: 'сталь', weight: '4' },
];
```

<details>
   <summary>Решение</summary>

```ts
const result = goods.reduce((acc, cur) => {
    const { id, type, weight } = cur;

    if (!acc[type]) {
        acc[type] = { ids: [], totalWeight: 0 };
    }

    acc[type].ids.push(id);
    acc[type].totalWeight += Number(weight);

    return acc;
}, {})
```
</details>



 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Есть массив операций. Необходимо операции отсортировать по дате и сгруппировать их по году, а в качестве значений представить массивы c датами в формате MM-DD.

```ts
result = {
   "2017": [
      "07-31",
      "08-22"
   ],
   "2018": [
      "01-01",
      "02-22"
   ]
}
```


```ts
const operations = [
    { "date": "2017-07-31", "amount": "5422" },
    { "date": "2017-06-30", "amount": "5220" },
    { "date": "2017-05-31", "amount": "5365" },
    { "date": "2017-08-31", "amount": "5451" },
    { "date": "2017-09-30", "amount": "5303" },
    { "date": "2018-03-31", "amount": "5654" },
    { "date": "2017-10-31", "amount": "5509" },
    { "date": "2017-12-31", "amount": "5567" },
    { "date": "2018-01-31", "amount": "5597" },
    { "date": "2017-11-30", "amount": "5359" },
    { "date": "2018-02-28", "amount": "5082" },
    { "date": "2018-04-14", "amount": "2567" }
];

function sortOperations(operations) {
    //ваш код здесь
}
```

<details>
   <summary>Решение</summary>

```ts
function sortOperations(operations) {
    return operations
        .sort((a, b) => {
            const dateA = new Date(a.date);
            const dateB = new Date(b.date);

            return dateA - dateB;
        })
        .reduce((acc, operation) => {
            const date = new Date(operation.date);
            const year = date.getFullYear();
            const month = date.getMonth() + 1;
            const day = date.getDate();

            if (!acc[year]) {
                acc[year] = [];
            }

            acc[year].push(`${month}-${day}`);

            return acc;
        }, {});
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Есть массив массивов. Первый массив - значения, второй массив - ключи

```ts

const data1 = [
        [1,2,3,4,5,6,7,8],
        ["name1","name2","name3","name4","name5","name6","name7","name8"]
    ]

```

Нужно сделать объект такой структуры:

```ts

const result = {
    name1: 1, 
    name2: 2,
    name3: 3,
     ...
    name8: 8,
}

```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Есть массив игроков. Нужно разделить их на тех, к кого есть команда и нет. Определяем наличие команды по полю `squad`.

```ts
const players = [
   { id: 2, squad: 1 },    
   { id: 3, squad: 1 },
   { id: 4, squad: null },
   { id: 5, squad: 2 },
   { id: 6, squad: 1 },
   { id: 7, squad: 2 }
]

const groupPlayersBySquad = (players) => {
    
}

const [playersWithSquad, playersWithoutSquad] = groupPlayersBySquad();


```

<details>
<summary>Решение</summary>

```ts
const groupPlayersBySquad = (players) => {
    let playersWithSquad = [];
    let playersWithoutSquad = [];

    for (var i = 0; i < players.length; i++) {
        if (players[i].squad !== null ) {
            playersWithoutSquad.push(players[i]);
        } else {
            playersWithSquad.push(players[i]);
        }
    }
    
    return [playersWithSquad, playersWithoutSquad];
}

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

#### Задача 1
Реализовать функцию getMoney для банкомата, выдавшего купюры.
На входе сумма, на выходе объект с количеством купюр по каждому номинал, при этом банкомат должен выдать
минимальное количество банкнот.

Доступные номиналы: 50, 100, 500, 1000, 5000р

```tsx
const getMoney = (amount) => {
    
}

console.log(getMoney(6200)) // { 5000: 1, 1000: 1, 500: 0, 100: 2, 50: 0 }
```

<details>
   <summary>Решение</summary>

```tsx
function getMoney(amount) {
    const nominals = [5000, 2000, 1000, 500, 100, 50];
    const result = {};

    for (let nominal of nominals) {
        if (amount === 0) break;

        const limit = limits[nominal];
        const count = Math.min(Math.floor(amount / nominal));
        amount = amount - (count * nominal);

        if (count) {
            result[nominal] = count;
        }
    }

    return result;
}
```
</details>


#### Задача 2

Добавить возмодность указывать, какие купюры есть в банкомате

```js
console.log(getMoney(6200, {5000: 0, 2000: 2, 1000: 7, 100: 5})) // {5000: 0, 1000: 6, 100: 2}
```

<details>
   <summary>Решение</summary>

```tsx
function getMoney(amount, limits = {}) {
    const nominals = [5000, 2000, 1000, 500, 100, 50];
    const result = {};

    for (let nominal of nominals) {
        if (amount === 0) break;

        const limit = limits[nominal];
        const count = Math.min(Math.floor(amount / nominal), limit) || 0;
        amount = amount - (count * nominal);

        if (count) {
            result[nominal] = count;
        }
    }

    return result;
}
```
</details>


 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### ✅ Задача

Написать класс EventEmmiter, аналог addeventlistener. 
- Метод метод on, который вызывается в именем события и функцией
- Метод off, который вызывается также с названием события и функцией
- Метод emit, который триггерит все функции

```ts

class EventEmitter {
  
}

// Пример использования
const myEventEmitter = new EventEmitter();

const greetListener = (name: string) => {
  console.log(`Hello, ${name}!`);
};

myEventEmitter.on('greet', greetListener);
myEventEmitter.emit('greet', 'Alice'); // Output: Hello, Alice!

myEventEmitter.off('greet', greetListener);
myEventEmitter.emit('greet', 'Bob'); // No output
```

<details>
  <summary>Решение</summary>

- Создаем поле класса, где будем хранить функции по каждому названию собития - это будет объект, типа `Record<string, Function[]>`
- В on, если по такому типа нет событий создается пустой массив и пушим в него функцию
- В off делаем фильтр и удаляем найденную функцию
- В emit проходим через forEach по функциям и вызываем их

```ts
class EventEmitter {
  private events: Record<string, Function[]> = {};

  on(eventName: string, listener: Function) {
      if (!this.events[eventName]) {
          this.events[eventName] = [];
      }
      this.events[eventName].push(listener);
  }

  off(eventName: string, listener: Function) {
      if (this.events[eventName]) {
          this.events[eventName] = this.events[eventName].filter((eventListener) => eventListener !== listener);
      }
  }

  emit(eventName: string, ...args: any[]) {
      if (this.events[eventName]) {
          this.events[eventName].forEach((listener) => {
              listener(...args);
          });
      }
  }
}

// Пример использования
const myEventEmitter = new EventEmitter();

const greetListener = (name: string) => {
  console.log(`Hello, ${name}!`);
};

myEventEmitter.on('greet', greetListener);
myEventEmitter.emit('greet', 'Alice'); // Output: Hello, Alice!

myEventEmitter.off('greet', greetListener);
myEventEmitter.emit('greet', 'Bob'); // No output
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

- Написать базовый/простой стейт менеджер, который может работать независимо от React
- Написать хуки для React для работы с этим стейт менеджером
- Связать кнопку со счетчиком

```tsx

class Store {
    
}

const useStore = () => {
    
}

export default function App() {
    const state = useStore(store);

    const handleClick = () => {
        
    }
    
    return (
        <div>
            <div>Counter из React: {state.value}</div>
            <button onClick={handleClick}>CLICK</button>
        </div>
    );
}
```


<details>
   <summary>Решение</summary>

```tsx
import { useEffect, useState } from "react";

class Store {
    constructor(initialState) {
        this.state = initialState;
        this.subscribers = [];
    }

    getState() {
        return this.state
    }

    setState(newState) {
        const resultState = {
            ...this.state,
            ...newState,
        }
        this.state = resultState
        this.subscribers.forEach(sub => {
            sub(resultState)
        })
    }

    subscribe(fn) {
        this.subscribers.push(fn);
    }

    unsubscribe(fn) {
        this.subscribers = this.subscribers.filter(subscriber => subscriber !== fn);
    }
}


const store = new Store({ value1: 0, value2: 0 })

const useStore = (store) => {
    const [localState, setLocalState] = useState(store.getState())

    useEffect(() => {
        store.subscribe(setLocalState)

        return () => store.unsubscribe();
    }, [])

    return localState;
}

export default function App() {
    const state1 = useStore(store);
    const state2 = useStore(store);

    const handleClick1 = () => {
        store.setState({ value1: state1.value1 + 1 });
    }
    const handleClick2 = () => {
        store.setState({ value2: state2.value2 + 1 });
    }

    return (
        <div>
            <div>Counter из React: {state1.value1}</div>
            <div>Counter из React: {state2.value2}</div>
            <button onClick={handleClick1}>CLICK</button>
            <button onClick={handleClick2}>CLICK</button>
        </div>
    );
}

```

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Сагрегировать данные для страницы постов
Список постов: https://jsonplaceholder.typicode.com/posts
Список пользователей: https://jsonplaceholder.typicode.com/users
Список комментариев: https://jsonplaceholder.typicode.com/comments

```ts
// Посты
// [{
//     "userId": 1,
//     "id": 1,
//     "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
//     "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
// }]

// Комментарии
//     [{
//     "postId": 1,
//     "id": 1,
//     "name": "id labore ex et quam laborum",
//     "email": "Eliseo@gardner.biz",
//     "body": "laudantium enim quasi est quidem magnam voluptate ipsam eos\n tempora quo necessitatibus\ndolor quam autem quasi\nreiciendis et nam sapiente accusantium"
// }]

// Пользователи
//     [{
//     "id": 1,
//     "name": "Leanne Graham",
//     "username": "Bret",
//     "email": "Sincere@april.biz",
//     "phone": "1-770-736-8031"
// }]

// Выходной формат данных (посты):
//     [{
//     "id": 1, // id поста
//     "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit", // title поста
//     "userName": "Leanne Graham",
//     "commentsCount": 10,
// }]

const getPosts = async () => {

}

getPosts();
```

<details>
   <summary>Решение</summary>

```ts
const getPosts = async () => {
   const requests = [
      'https://jsonplaceholder.typicode.com/posts',
      'https://jsonplaceholder.typicode.com/users',
      'https://jsonplaceholder.typicode.com/comments'
   ].map((url) => fetch(url));

   const [posts, users, comments] = await Promise.all(requests);
    
    const commentsByPost = comments.reduce((res, comment) => {
        if (!res[comment.postId]) {
            res[comment.postId] = 0
        }
        res[comment.postId] += 1
        return res
    }, {})

    const usersById = users.reduce((res, user) => {
        res[user.id] = user.name
        return res
    }, {})

    return posts.map(post => {
        const commentsCount = commentsByPost[post.id] || 0
        const userName = usersById[post.userId]
        return {
            id: post.id,
            title: post.title,
            userName,
            commentsCount,
        }
    })
}

```

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Реализовать очередь, позволяющую выполнить проверочный код
Первым аргументом конструктора является функция — обработчик одной задачи, задача считается обработанной после вызова коллбэка
Вторым — количество параллельно обрабатываемых задач
Третьим — коллбек, вызываемый по окончанию обработки всех задач

```ts
class Queue {
    
}

const processTask = (task, resolve) => {
  // время обработки задачи от 500 до 1000мс
  const workTime = 500 + Math.floor(Math.random() * 500)

  setTimeout(() => {
    console.log(task)
    resolve()
  }, workTime)
}

const paralleledTasks = 2
const whenEmpty = () => console.log('Queue is empty')

const queue = new Queue(processTask, paralleledTasks, whenEmpty)

queue.add('task 1')
queue.add('task 2')
queue.add('task 3')
queue.start()

/*
example output:
task 2
task 1
task 3
Queue is empty

* поскольку задачи обрабатываются
* разное количество времени, то при
* paralleledTasks > 1 вывод может быть
* не последовательным как тут: сначала
* успела выполниться task 2, а потом task 1
*/

```

<details>
   <summary>Решение</summary>

```ts

class Queue {
    constructor(processTask, paralleledTasks, endCb) {
        this.tasks = [];
        this.workersNum = paralleledTasks;
        this.endCb = endCb;
        this.processTask = processTask;
    }

    add(task) {
        this.tasks.push(task);
    }

    async runWorker() {
        while (this.tasks.length > 0) {
            const task = this.tasks.shift();
            if (task) {
                await new Promise(resolve => this.processTask(task, resolve));
            }
        }
    }

    async loop() {
        if (this.tasks.length === 0) {
            this.endCb();
            return;
        }

        const workers = Array.from({ length: this.workersNum }, () => this.runWorker());
        await Promise.all(workers);

        this.endCb();
    }
}

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Реализовать функцию spyOn
Функция должна позволять отслеживать вызовы оригинальной функции, не заменяя при этом её поведение.
Похожую функцию можно встретить во фреймворках для написания тестов, например jest.spyOn.
Для хранения аргументов отслеживаемых вызовов необходимо использовать массив calls.

```ts

function spyOn(obj, key) {
  // Реализация
}

const person = {
  firstName: '',
  lastName: '',
  update(fullName) {
    const [firstName, lastName] = fullName.split(' ');
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

const spy = spyOn(person, 'update');

person.update('Иван Иванов');
console.log(person.firstName, person.lastName); // Иван Иванов

person.update('Пётр Петров');
console.log(person.firstName, person.lastName); // Пётр Петров

console.log(spy.calls); // [['Иван Иванов'], ['Пётр Петров']]

```

<details>
   <summary>Решение</summary>

```ts
function spyOn(obj, key) {
    const self = obj[key];
    let calls = [];
    obj[key] = function (...args) {
        self.call(obj, ...args);
        calls.push(args);
    }

    return {
        calls,
    }
}
```

</details>
   
 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

Реализовать функцию, возвращающую все свободные временные слоты для создания встречи нескольких участников.
Функция должна вернуть все свободные слоты в промежутке от 0 до 24, общие для всех участников встречи.
Все временные интервалы указаны в виде массива чисел [start, end], где start всегда меньше end.

```ts
function findFreeMeetingSlots(slots) {

}

console.log(findFreeMeetingSlots([
    [[16, 18], [11, 12], [14, 15]], // занятые слоты участника 1
    [[15, 17]],                     // занятые слоты участника 2
    [[10, 13]]                      // занятые слоты участника 3
]))
// [[0, 10], [13, 14], [18, 24]]

console.log(findFreeMeetingSlots([
    [[11, 12.34], [14.54, 19], [19.3, 20]],   // занятые слоты участника 1
    [[15.45, 16], [17, 19.10]],              // занятые слоты участника 2
    [[10, 13]]                               // занятые слоты участника 3
]))
// [[0, 10], [13, 14.54], [19.13, 19.3], [20, 24]]
```

<details>
   <summary>Решение</summary>

```ts
function findFreeMettingSlots(slots) {
  const sortedSlots = slots.flat().sort((a, b) => a[0] > b[0] ? 1 : -1);

  /*
    Получаем слоты, отсортированные по началу встречи:
    [
      [10, 13],
      [11, 12],
      [14, 15],
      [15, 17],
      [16, 18],
    ]
  */

  const freeSlots = [];

  // Заводим указатель, который указывает время окончания последнего занятого слота
  let lastEnd = 0;

  for (const slot of sortedSlots) {
    if (lastEnd < slot[0]) {
      freeSlots.push([lastEnd, slot[0]]);
    }
    lastEnd = Math.max(lastEnd, slot[1]);
  }

  if (lastEnd < 24) {
    freeSlots.push([lastEnd, 24]);
  }

  return freeSlots;
}
```

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

```ts

// Дан массив коннектов, каждый содержит путь от от до до. Необходимо обойти все коннекты и построить самые длинные цепочки.
// В рамках задачи считаем, что зацикленных путей быть не может.

const connections = [
    ['A', 'Z'], ['Z', 'K'], ['L', 'A'], ['L', 'O'],
    ['J', 'E'], ['Z', 'J'], ['Q', 'E'], ['Y', 'L']
]

const result = [
    ['Q', 'E'],
    ['Y', 'L', 'A', 'Z', 'K'],
    ['Y', 'L', 'A', 'Z', 'J', 'E'],
    ['Y', 'L', 'O']
]

```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

<details>
    <summary>Решение</summary>
    
 ```ts
 const getLongestChainedConnections = (connections: Array<[string, string]>): Array<Array<string>> => {
   const {
      connectionsByFrom,
      connectionsByTo,
   } = connections.reduce<{ connectionsByFrom: Record<string, Array<string>>, connectionsByTo: Record<string, Array<string>> }>((acc, [from, to]) => {
      if (!acc.connectionsByFrom[from]) {
         acc.connectionsByFrom[from] = []
      }

      if (!acc.connectionsByFrom[from].includes(to)) {
         acc.connectionsByFrom[from].push(to)
      }

      if (!acc.connectionsByTo[to]) {
         acc.connectionsByTo[to] = []
      }

      if (!acc.connectionsByTo[to].includes(from)) {
         acc.connectionsByTo[to].push(from)
      }

      return acc
   }, {
      connectionsByFrom: {},
      connectionsByTo: {},
   })

   const startedNodes = Object.keys(connectionsByFrom).reduce((acc, from) => {
      if (!connectionsByTo[from]?.length) {
         acc.push(from)
      }
      return acc
   }, Array<string>())

   const getChained = (acc: string[][], from: string, index: number): string[][] => {
      if (connectionsByFrom[from]?.length) {
         if (connectionsByFrom[from].length > 1) {
            const copy = [...acc[index]]

            connectionsByFrom[from].forEach((to, toIndex) => {
               if (!toIndex) {
                  const to = connectionsByFrom[from][0]
                  acc[index].push(to)
                  getChained(acc, to, index)
               } else {
                  const newIndex = acc.length
                  acc[newIndex] = copy
                  acc[newIndex].push(to)
                  getChained(acc, to, newIndex)
               }
            })
         } else {
            const to = connectionsByFrom[from][0]
            acc[index].push(to)
            getChained(acc, to, index)
         }
      }

      return acc
   }

   return startedNodes.reduce(getChained, startedNodes.map((n) => [n]))
}
 ```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

```ts
declare global {
    var RendererFunc: RendererFunc
}

interface RendererFunc {
    new(): RendererFunc
    init: Function
}


export interface ConfInterface {
    lang: string,
    target: HTMLElement,
    themeName: string,
    onLogin: Function,
    onRegister: Function
}

// Скрипт содержит экземпляр RendererFunc
export function loadScript(callback) {
    if (!document.getElementById('FOO_SCRIPT_ID')) {
        const script = document.createElement('script')

        script.id = 'FOO_SCRIPT ID'
        script.src = 'https://foo.com/foo.script.js'

        document.head.appendChild(script)
        script.onload = callback

        return
    }

    callback()
}

export function initScript (
    lang: string,
    frameKey: string,
    onLogin: Function,
    onRegister: Function,
    stickyTop: number,
    themeName: string,
    optionsList: unknown,
    onNavigation
): any {
    let target = document.getElementById( '#wrapper')
    let options = [];

    optionsList.forEach((i) => {
        if (options.find(({code}) => code === i.id)) {
            return;
        }

        const op = {
            code: i.id,
            label: i.cardType,
            value: i.title,
            dataTest: i.id
        }

        options.push(op);
    });

    const conf: ConfInterface = {
        lang: lang,
        onLogin: onLogin,
        stickyTop: stickyTop ?? 70,
        target,
        themeName: themeName || 'defaultTheme',
        options

    }

    function onNavigation(e?: any): void {
        let url = '';

        const name = e.pageName.toLowerCase();

        if (name == 'detasils') {
            url = '#events';
        } else if (name === 'coming') {
            url = '#calendar';
        } else if (name == 'results') {
            url = '#results'
        } else if (name === 'live') {
            url = '#live';
        }

        // Установка человекочитаемой ссылки
        window.history.replaceState(null, '', url);
    }

    conf.onNavigation = onNavigation

    if (onRegister) {
        conf.onRegister = onRegister
    }

    if (window.RendererFunc && target) {
        return new RendererFunc().init(conf);
    }
}
```

<details>
   <summary>Решение</summary>

```ts
// types.ts
interface OptionElement {
    code: string;
    label: string
    value: string;
    dataTest: string;
}

interface OptionListElement {
    readonly id: string;
    cardType: string
    title: string
}

export interface ConfInterface {
    lang: string,
    target: HTMLElement,
    themeName: string,
    stickyTop: number;
    onLogin: () => void,
    onRegister: () => void
    options: OptionElement[]
}

interface RendererFunc {
    new(): RendererFunc
    init: (config: ConfInterface) => void
}

declare global {
    const RendererFunc: RendererFunc
}

// src/herlpers/script/load.ts
const RENDERER_SCRIPT_ID = 'FOO_SCRIPT_ID';
const RENDERER_SCRIPT_URL = 'https://foo.com/foo.script.js';

// Скрипт содержит экземпляр RendererFunc
export function loadScript(callback: VoidFunction) {
    if (document.getElementById(RENDERER_SCRIPT_ID)) {
        callback();
        return;
    }

    const script = document.createElement('script')

    script.id = RENDERER_SCRIPT_ID
    script.src = RENDERER_SCRIPT_URL

    document.head.appendChild(script)
    script.onload = callback
}


const prepareOptions = (optionsList: OptionListElement[]): OptionElement[] => {
    const optionsSet = new Set<string>();
    const options: OptionElement[] = [];

    for (let i = 0; i < optionsList.length; i++) {
        const { id, title, cardType } = optionsList[i];

        if (optionsSet.has(id)) {
            continue;
        }

        optionsSet.add(id);

        return {
            code: id,
            label: cardType,
            value: title,
            dataTest: id
        }
    }

    return options;
}

const navigationMap = {
    detasils: '#events',
    coming: '#calendar',
    results: '#results',
    live: '#live',
}

function onNavlinkNavigation(e?: Event): void {
    const name = e.pageName.toLowerCase();
    const url = navigationMap[name] || '';

    window.history.replaceState(null, '', url);
}

interface InitScriptParams {
    lang: string,
    stickyTop: number,
    themeName: string,

    optionsList: OptionListElement[],
    onLogin: () => void,
    onRegister?: () => void,
    onNavigation: (event?: Event) => void
}

export function initScript (params: InitScriptParams) {
    const {
        lang,
        stickyTop,
        themeName,
        optionsList = [],
        onLogin,
        onRegister,
        onNavigation = onNavlinkNavigation,
    } = params;

    const target = document.getElementById( 'wrapper');

    const conf: ConfInterface = {
        lang,
        target,
        stickyTop: stickyTop ?? 70,
        themeName: themeName || 'defaultTheme',
        onLogin,
        options: prepareOptions(optionsList),
        onNavigation
    }

    if (onRegister) {
        conf.onRegister = onRegister
    }

    if (window.RendererFunc && target) {
        return new RendererFunc().init(conf);
    }
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### ✅ Задача

Сделать функцию-обертку для кэширования, которая не будет повторно вызывать фунцию при передаче одних и тех же параметров

```ts
function cache(callback) {
}

const getSum = (a, b) => {
  console.log(':: calculation');
  return a + b;
};
const cachedSum = cache(getSum);

console.log(cachedSum(1, 2)); // должен быть вызов sum
console.log(cachedSum(2, 3)); // должен быть вызов sum
console.log(cachedSum(1, 2)); // не вызываем функцию sum и берем значение из кэша
console.log(cachedSum(2, 3)); // не вызываем функцию sum и берем значение из кэша
```

<details>
 <summary>Решение</summary>

 Кэширование будет работать, но `'2'` и 2 будут считаться одинаковым значением

```ts
function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = args.join('_');

		if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    } 

    return cache[key];
  }
}

```

Кэширование разных по типу, но одинаковых по значению параметров

```ts
console.log(cachedSum(1, 2));
console.log(cachedSum(1, 2));

function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = args.sort().join('_');

    if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    }

    return cache[key];
  };
}
```

Если нужна возможность передавать не только примитивы, но и объекты, то нужно серриализовать объект с помощью JSON.stringify

```ts

function cache(callback) {
  const cache = {};

  return function(...args) {
    const key = JSON.stringify(args);

    if (!cache[key]) {
      const result = callback(...args);
      cache[key] = result;
    }

    return cache[key];
  };
}

const getSum = (a, b) => {
  console.log(':: calculation');
  return a.a + b.b;
};

const cachedSum = cache(getSum);

cachedSum({ a: 1 }, { b: 2 }); // no cache
cachedSum({ a: 2 }, { b: 3 }); // no cache
cachedSum({ a: 1 }, { b: 2 }); // cache
cachedSum({ a: 2 }, { b: 3 }); // cache
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Сделать вызов функции sum с двумя значениями через `(param1, param2)` и `(param1)(param2)` способоами с использованием каррирования

```ts
const sum = (val1, val2) => {

}

sum(1, 2);
sum(1)(2);
```

<details>
  <summary>Решение</summary>

```ts
const sum = (val1, val2) => {
	if (val2) {
    return val1 + val2;
  }
  
  return (_val2) => val1 + _val2;
}
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

Продолжение предыдущей задачи  
Сделать каррирование с возможность передавать бесконечное количество параметров

```ts
const sum = () => {
  
};

sum(1)(); // 1
sum(1)(2)(3)(); // 6
```

<details>
  <summary>Решение</summary>
 
```ts
const sum = (a) => {
  let res = a;
  const cb = (b) => {
    if (b === undefined) {
      return res;
    }
    res += b;

    return cb;
  };

  return cb;
};

console.log(sum(1)(3)(5)());

```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

 
### ✅ Задача

Задача: Создайте игру "Память", в которой пользователи должны находить пары совпадающих карточек.

Правила:
1. Пользователям представляют 10 пар совпадающих карточек (всего 20 карточек) в случайном порядке.
2. Карточки отображаются в течение 3 секунд, затем все они переворачиваются, скрывая значения.
3. Пользователи могут переворачивать по 2 карточки за раз:
   - Если карточки совпадают, они должны показывать совпадение и остаться перевёрнутыми.
   - Если карточки не совпадают, они должны быть перевернуты обратно.
4. Пользователь продолжает этот процесс, пока все пары карточек не будут отгаданы.

Задачи для выполнения:
1. Отобразите на экране 10 пар совпадающих карточек в случайном порядке.
2. Реализуйте "переворачивание" карточек.
3. Отобразите карточки (лицом вверх) в течение 3 секунд в начале и затем переверните их все лицом вниз.
4. Разрешите переворачивать не более двух карточек за раз и:
   - если они совпадают, показывайте совпадение и оставляйте их перевёрнутыми.
   - если они не совпадают, переверните их обратно.
5. Кнопка перезапуска должна перезапустить игру.

[Архив](resources/memory-game/Task.zip) (Чтобы скачать файл, нажмите ... справа сверху -> Download)

<details>
  <summary>Решение</summary>

  [Итоговый архив](resources/memory-game/Result.zip) (Чтобы скачать файл, нажмите ... справа сверху -> Download)
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

 
### Задача

Такую задачу невозможно решить самому. Тут используется [Польская нотация](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%BB%D1%8C%D1%81%D0%BA%D0%B0%D1%8F_%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C). ГПТ генерирует решение на 100+ строк.    
Только на одном собеседовании давали такую задачу, ее можно посмотреть для общего развития, но серьезно готовиться к ней не стоит

```ts
// Your job is to create a calculator which evaluates expressions in Reverse Polish notation.
// For example expression 5 1 2 + 4 * + 3 - (which is equivalent to 5 + ((1 + 2) * 4) - 3 in normal notation) should evaluate to 14.
// For your convenience, the input is formatted such that a space is provided between every token.
// Empty expression should evaluate to 0.
// Valid operations are +, -, *, /.
// You may assume that there won't be exceptional situations (like stack underflow or division by zero).

const calcPN = (str) => {
  // TODO: Your awesome code here
};

calcPN("2 3 *"); // 6
calcPN("5 1 2 + 4 * + 3 -"); // 14
// STEPS:
// 5 1 2 + 4 * + 3 - || 1 + 2 => 3
// 5 3 4 * + 3 - || 3 * 4 => 12
// 5 12 + 3 - || 5 + 12 => 17
// 17 3 - || 17 - 3 => 14
// 14
```

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
 
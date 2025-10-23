# Задачи для практики к собеседованиям на frontend-разработчика - Promise

## Promise

### ✅ Задача

Написать функцию для задержки выполнения кода

```ts
delay(1000).then(() => alert('Hello!'));
```

<details>
    <summary>Решение</summary>

```ts
async function delay (ms) {
    return new Promise ((resolve) => {
        setTimeout(resolve, ms)
    })   
}
```
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Когда в консоли выведится "timeout!"

```ts
function loop() {
  Promise.resolve().then(loop);
}

loop();

setTimeout(() => console.log('timeout!'), 0);
```

<details>
    <summary>Ответ</summary>

Ответ: Никогда

Если микротаски порождают новые микротаски бесконечно, то цикл событий никогда не дойдёт до макротасок.

Иными словами — макротаски “зависнут” навсегда.
JavaScript просто не перейдёт к следующей фазе (например, setTimeout не выполнится).
</details>

---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->




### ✅ Задача

Сделать аналог Promise.all и Promise.allSettled

```ts
const resolveFunc = async (val, timeout) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(val);
    }, timeout);
  });
};

const rejectFunc = async (val, timeout) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject(val);
    }, timeout);
  });
};

const requests = [
	resolveFunc(1, 100), 
	resolveFunc(2, 900), 
	resolveFunc(3, 800), 
	resolveFunc(4, 1500), 
	resolveFunc(5, 3500), 
	resolveFunc(6, 1500)
];

promiseAll(requests)
  .then(console.log)
  .catch(console.error);
  
promiseSettled(requests)
  .then(console.log)
  .catch(console.error);
```

<details>
  <summary>Решение</summary>

Promise.all
```ts
const promiseAll = async promises => {
  return new Promise((resolve, reject) => {
    const result = new Array(promises.length);

    let finishedPomisesCount = 0;

    for (let index in promises) {
      promises[index]
        .then(data => {
          finishedPomisesCount++;
          result[index] = data;

          if (promises.length === finishedPomisesCount) {
            resolve(result);
          }
        })
        .catch(data => {
          reject(data);
        });
    }
  });
};

promiseAll(requests)
  .then(console.log)
  .catch(console.error);

```

Promise.allSettled
```ts
const promiseAllSettled = async promises => {
  return new Promise((resolve, reject) => {
    const result = new Array(promises.length);

    let finishedPomisesCount = 0;

    for (let index in promises) {
      let tempPromiseResult = null;
      promises[index]
        .then(data => {
          tempPromiseResult = { status: 'fulfilled', data };
        })
        .catch(data => {
          tempPromiseResult = { status: 'rejected', data };
        })
        .finally(() => {
          result[index] = tempPromiseResult;
          finishedPomisesCount++;

          if (promises.length === finishedPomisesCount) {
            resolve(result);
          }
        });
    }
  });
};
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->




### ✅ Задача

Есть GET запрос https://jsonplaceholder.typicode.com/posts/ID, который возвращает JSON вида:
```json
{
   "userId": 5,
   "id": 42,
   "title": "commodi ullam sint et excepturi error explicabo praesentium voluptas",
   "body": "odio fugit voluptatum ducimus earum autem est incidunt voluptatem"
}
```

Нужно сделать функцию, которая принимает массив ID постов, параллельно их запрашивает и отдает результат

```ts
const fetchData = async (ids) => {
    const requests = ids.map(id => {
        return fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
            .then(response => response.json())

    })

    return Promise.allSettled(requests);
};

fetchData([1, 2, 3])
    .then(value => console.log(value))
    .catch(err => console.log(err));

```

<details>
  <summary>Решение</summary>

```ts
interface Data {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const fetchData = async(array: number[]): Promise<Data[]> => {
  const res = array.map(num => fetch(`https://jsonplaceholder.typicode.com/posts/${num}`).then(res => res.json()))

  const response = await Promise.all(res);

  return response
}

fetchData([42, 2, 3]).then(value => console.log(value)).catch(err => console.log(err))
```

</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->



### ✅ Задача

Получние данных с количестом попыток на перезапрос. Реализовать функцию, которая будет делать сетевой запрос. Если он завершается с ошибкой, то повторяет его через секунду.  
После трех неудачных попыток - завершается ошибкой. Или возвращает успешный результат.  

```ts
async function runWithRetry (url, times = 3) {

runWithRetry('https://jsonplaceholder.typicode.com/posts/1')
    .then(console.log)
    .catch(console.error);
```

<details>
    <summary>Решение</summary>

```ts

async function runWithRetry1(url, times) {
    return new Promise(async (resolve, reject) => {
        for (let time of times) {
            const result = await fetch(url);

            if (result.ok) {
                resolve(result.json());
            }

            await sleep(time * 1000)
        }

        reject('Бекенд не отвечает')
    })
}

// ИЛИ

async function runWithRetry2(url, times = 3) {
    return await fetch(url)
        .then(res => {
            if (!res.ok) {
                throw new Error(res.statusText)
            }
            return res.json()
        })
        .catch(async () => {
            if (times - 1) {
                await sleep(1000);
                return await runWithRetry(url, times - 1);
            } else {
                throw new Error('Бекенд не отвечает');
            }
        });
}

const sleep = async (time) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve()
        }, time)
    })
}

runWithRetry('https://jsonplaceholder.typicode.com/posts/1')
    .then(console.log)
    .catch(console.error)


```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

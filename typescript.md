# Задачи для практики к собеседованиям на frontend-разработчика - TypeScript

## Тех. Вопросы

- В чем минус использования TypeScript?
- Разница типов и интерфейсов
- Что такое дженерик
- Что такое декларейшен
- Что такое infer и conditional types
- Какие утилити тайпы ты знаешь
- never vs any vs unknown

## Лайф-кодинг

### ✅ Задача

Написать свою реализацию Partial (Объект, где все поля опциональные)   

```ts
type User = {  
  id: number  
}  

type PartialUser = NewPartial<User>;

type NewPartial = ....
```

<details>
  <summary>Решение</summary>

```ts
type NewPartial<T> = {  
  [key in keyof T]?: T[key];  
}
```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Записать правильный тип для MYType, чтоб переменные, которым это тип будет присвоен имели тип состоящий из ключей объекта obj

```ts
const obj = {
  name: "Nik",
  age: 25,
};

type MYType = any;
// Вместо any нужный тип

//---------

/** Тут не должно быть ошибок типов */

const var1: MYType = "name";

const var2: MYType = "age";

//----------

/** Тут должны быть ошибки типов */

const var3: MYType = "test";

const var4: MYType = 25;

```

<details>
  <summary>Решение</summary>

```ts
  type MYType = keyof typeof obj // "name" |"age"
```
</details>

  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


 ### ✅ Задача

 Написать generic Pick

 ```ts
type User = {
  id: number;
  name: string;
  surname: string;
}

type RequiredUser = NewPick<User, 'id' | 'name'>

type NewPick = ....

```

<details>
  <summary>Решение</summary>

```ts
type NewPick<T, K extends keyof T> = {
    [P in K]: T[P];
}
```
</details>






  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Есть заранее описанный объект бесконечной вложенности. Конечные свойства всегда `string`. Нужно написать тип, который будет давать значения-пути для ключа конечного свойства типа `string`.

 ```json
{
    "greeting": {
        "hello": "Hello",
            "goodbye": "Goodbye"
    },
    "user": {
        "profile": "Profile",
            "settings": "Settings"
    }
}
```

  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### Задача

Реализовать ResolvableKeysOf

```ts
class Test {
   public a = 1;
   public b = 2;
   public c = Promise.resolve(3);
   public d = () => 5;
   public e = () => Promise.resolve(6);


   public x = 'not';
   public y = Promise.resolve(null);
   public z = () => true;
}


type NumberKeys = ResolvableKeysOf<Test, number>; // "a" | "b" | "c" | "d" | "e"
type StringKeys = ResolvableKeysOf<Test, string>; // "x"
type NullKeys = ResolvableKeysOf<Test, null>; // "y"
type BooleanKeys = ResolvableKeysOf<Test, boolean>; // "z"
type StringOrNullKeys = ResolvableKeysOf<Test, string | null>; // "x" | "y"
```

  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### Задача

Типизировать функцию

```ts
const someObject = { a: 1, b: 2, c: 3, d: 4, }
const someObject2 = { m: 1, b: 2, c: 3, d: 4, }

const getProperty = (obj, key) => {
    return obj[key];
}

getProperty(someObject, 'a') // OK
getProperty(someObject, 'm') // ERROR
getProperty(someObject2, 'm') // OK
```

  ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
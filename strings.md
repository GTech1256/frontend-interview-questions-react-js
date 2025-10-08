# Задачи для практики к собеседованиям на frontend-разработчика - Работа со строками

## Работа со строками

### ✅ Задача

Написать функцию, возвращающую предложение, в котором каждое второе слово читается в обратном порядке.
Знаки препинания, специальные знаки, числа и пробелы остаются на своих местах.

```ts
console.log(evenBack('Look at the sky'));
console.log(evenBack('21 plus 22 = sorok tri'));

console.log(evenBack('Look at the sky') === 'Look ta the yks');
console.log(evenBack('21 plus 22 = sorok tri') === '21 plus 22 = koros tri');


function evenBack(string) {
  
}
```

<details>
  <summary>Решение</summary>

```ts

function reverseString(string) {
  let reversed = '';

  for (let i = string.length - 1; i >= 0; i--) {
    reversed += string[i];
  }

  return reversed;
}

function evenBack(string) {
  const words = string.split(' ');

  let wordsIndex = 0;
  const modifiedWords = words.map(word => {
    if (/^[a-zA-Z]+$/.test(word)) {
      wordsIndex++;
    }

    if (wordsIndex % 2 === 0) {
      return reverseString(word);
    }

    return word;
  });

  return modifiedWords.join(' ');
}

```
</details>



 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Вывести строки, в которых есть подстрока

```ts
const findSubstring = (substribg, arr) => {};

console.log(
  findSubstring("am", [
    "fuzzy",
    "maskva",
    "mama",
    "search",
    "algorithm",
    "utility",
  ])
);

```

<details>
  <summary>Решение</summary>

```ts
const fuzzySearch = (substring, arr) => {
  return arr.filter(string => {
    let index = 0;

    for (let i = 0; i < string.length; i++) {
      if (string[i] === substring[index]) {
        index++;
      } else {
        index = 0;
      }

      if (index === substring.length) {
        return true;
      }
    }

    return false;
  });
};

console.log(
  fuzzySearch('am', [
    'fuzzy',
    'maskva',
    'mama',
    'search',
    'am',
    'utility'
  ])
);
```
</details>
 
 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->

### ✅ Задача

Напишите функцию capitalize(input), возвращающую копию строки input, в которой каждое слово начинается с заглавной буквы.  
 
"Слово" в данном контексте - последовательность юникод-символов из группы "letters".  
В целях упрощения в тестовых кейсах будут использоваться только строки из латинских букв и кириллицы. Слова с дефисами ("Что-то", "кто-либо" и т.д.) считаются одним словом. 

```ts
function capitalize(input) {
  
}

console.log(capitalize('А роза упала на лапу Азора') === 'А Роза Упала На Лапу Азора');
```

<details>
  <summary></summary>

```ts

function capitalize(input) {
  return input
    .split(' ')
    .map(word => {
      const [first, ...rest] = word.split('');
      return first.toUpperCase() + rest.join('');
    })
    .join(' ');
}

console.log(capitalize('А роза упала на лапу Азора') === 'А Роза Упала На Лапу Азора');
```
</details>
 
 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->


### ✅ Задача

```ts
// Проверить, что все элементы подстроки встречаются в строке в таком же порядке

function needleInHaystack(substring, string) {
   
}

console.log(needleInHaystack('whe', 'cartwheel')) // true
console.log(needleInHaystack('crt', 'cartwheel')) // true
console.log(needleInHaystack('ctr', 'cartwheel')) // false
console.log(needleInHaystack('weee', 'cartwheel')) // false

```

<details>
  <summary>Решение</summary>

```ts
function needleInHaystack(substring, string) {
    let index = 0;
    
    for (let i = 0; i < string.length; i++) {
        if (string.charAt(i) === substring.charAt(index)) {
            index++;
        }
        if (index === substring.length) {
            return true;
        }
    }
    
    return false;
}

console.log(needleInHaystack('whe', 'cartwheel')) // true
console.log(needleInHaystack('crt', 'cartwheel')) // true
console.log(needleInHaystack('ctr', 'cartwheel')) // false
console.log(needleInHaystack('weee', 'cartwheel')) // false

```
</details>

 ---
 <!--  ------------------------------------------------------------------------------------------------------------------------------------------------------- -->
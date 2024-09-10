Допустим, у нас есть сложный объект, и мы хотели бы преобразовать его в строку, чтобы отправить по сети или просто вывести для логирования.

JSON - это общий формат для представления значений и объектов.

JavaScript предоставляет методы:
- `JSON.stringify` для преобразования объектов в JSON.
- `JSON.parse` для преобразования JSON обратно в объект.
```JS
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

  

let json = JSON.stringify(student);

alert(typeof json); // мы получили строку!
alert(json);

/* выведет объект в формате JSON:

{Допустим, у нас есть сложный объект, и мы хотели бы преобразовать его в строку, чтобы отправить по сети или просто вывести для логирования.

  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}

*/
```

Полученная строка `json` называется _JSON-форматированным_ или _сериализованным_ объектом. Мы можем отправить его по сети или поместить в обычное хранилище данных.

Обратите внимание, что объект в формате JSON имеет несколько важных отличий от объектного литерала:
- Строки используют двойные кавычки. Никаких одинарных кавычек или обратных кавычек в JSON. Так `'John'` становится `"John"`.
- Имена свойств объекта также заключаются в двойные кавычки. Это обязательно. Так `age:30` становится `"age":30`.

### `JSON.stringify` 
может быть применён и к примитивам.

JSON поддерживает следующие типы данных:

- Объекты `{ ... }`
- Массивы `[ ... ]`
- Примитивы:
    - строки,
    - числа,
    - логические значения `true/false`,
    - `null`.
```JS
// число в JSON остаётся числом
alert( JSON.stringify(1) ) // 1
// строка в JSON по-прежнему остаётся строкой, но в двойных кавычках
alert( JSON.stringify('test') ) // "test"
alert( JSON.stringify(true) ); // true
alert( JSON.stringify([1, 2, 3]) ); // [1,2,3]
```

JSON является независимой от языка спецификацией для данных, поэтому `JSON.stringify` пропускает некоторые специфические свойства объектов JavaScript.

А именно:

- Свойства-функции (методы).
- Символьные ключи и значения.
- Свойства, содержащие `undefined`.

```JS
let user = {
  sayHi() { // будет пропущено
    alert("Hello");
  },
  [Symbol("id")]: 123, // также будет пропущено
  something: undefined // как и это - пропущено

};

alert( JSON.stringify(user) ); // {} (пустой объект)
```

___Самое замечательное, что вложенные объекты поддерживаются и конвертируются автоматически.___

__Важное ограничение: не должно быть циклических ссылок. Иначе будет ошибка__

```JS
let room = {
  number: 23
};

  

let meetup = {
  title: "Conference",
  participants: ["john", "ann"]
};
  

meetup.place = room;       // meetup ссылается на room
room.occupiedBy = meetup; // room ссылается на meetup

JSON.stringify(meetup); 
// Ошибка: Преобразование цикличной структуры в JSON
```

Полный синтаксис `JSON.stringify`:
```JS
let json = JSON.stringify(value, [replacer, space])
```
`value`
Значение для кодирования.

`replacer`
Массив свойств для кодирования или функция соответствия `function(key, value)`.

`space`
Кол-во пробелов, используемое для форматирования.

```JS
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup ссылается на room
};

room.occupiedBy = meetup; // room ссылается на meetup

alert( JSON.stringify(meetup, function replacer(key, value) {
  alert(`${key}: ${value}`);
  return (key == 'occupiedBy') ? undefined : value;
}));

  

/* пары ключ:значение, которые приходят в replacer:
:             [object Object]
title:        Conference
participants: [object Object],[object Object]
0:            [object Object]
name:         John
1:            [object Object]
name:         Alice
place:        [object Object]
number:       23
occupiedBy: [object Object]
*/
```

Обратите внимание, что функция `replacer` получает каждую пару ключ/значение, включая вложенные объекты и элементы массива. И она применяется рекурсивно. Значение `this` внутри `replacer` – это объект, который содержит текущее свойство.

Первый вызов – особенный. Ему передаётся специальный «объект-обёртка»: `{"": meetup}`. Другими словами, первая `(key, value)` пара имеет пустой ключ, а значением является целевой объект в общем. Вот почему первая строка из примера выше будет `":[object Object]"`.

Идея состоит в том, чтобы дать как можно больше возможностей `replacer` – у него есть возможность проанализировать и заменить/пропустить даже весь объект целиком, если это необходимо.
### `JSON.parse`

```JS
let value = JSON.parse(str, [reviver])
```

`str`
JSON для преобразования в объект.

`reviver`
Необязательная функция, которая будет вызываться для каждой пары `(ключ, значение)` и может преобразовывать значение.

```JS
let numbers = "[0, 1, 2, 3]"

numbers = JSON.parse(numbers);
alert(numbers[1]); // 1
```

```JS
let user = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

user = JSON.parse(user);
  
alert( user.friends[1] ); // 1
```

## Типичные ошибки

Вот типичные ошибки в написанном от руки JSON (иногда приходится писать его для отладки):

```JS
let json = `{
  name: "John",                     
  // Ошибка: имя свойства без кавычек
  "surname": 'Smith',               
  // Ошибка: одинарные кавычки в значении (должны быть двойными)
  'isAdmin': false,                 
  // Ошибка: одинарные кавычки в ключе (должны быть двойными)
  "birthday": new Date(2000, 2, 3), 
  // Ошибка: не допускается конструктор "new", только значения
  "gender": "male"                  
  // Ошибка: отсутствует запятая после непоследнего свойства
  "friends": [0,1,2,3],             
  // Ошибка: не должно быть запятой после последнего свойства
}`;
```

## Использование reviver
Представьте, что мы получили объект `meetup` с сервера в виде строки данных.

Вот такой:
```JS
// title: (meetup title), date: (meetup date)
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';
```

…А теперь нам нужно _десериализовать_ её, т.е. снова превратить в объект JavaScript.

Давайте сделаем это, вызвав `JSON.parse`:
```JS
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str);

alert( meetup.date.getDate() ); // Ошибка!
```

Ой, ошибка!

Значением `meetup.date` является строка, а не `Date` объект. Как `JSON.parse` мог знать, что он должен был преобразовать эту строку в `Date`?

Давайте передадим `JSON.parse` функцию восстановления вторым аргументом, которая возвращает все значения «как есть», но `date` станет `Date`:
```JS
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;

});

alert( meetup.date.getDate() ); // 30 - теперь работает!
```
__Кстати, это работает и для вложенных объектов__

[[Symbol]]
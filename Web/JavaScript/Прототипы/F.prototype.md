Как мы помним, новые объекты могут быть созданы с помощью функции-конструктора `new F()`.

Если в `F.prototype` содержится объект, оператор `new` устанавливает его в качестве `[[Prototype]]` для нового объекта.

Обратите внимание, что `F.prototype` означает обычное свойство с именем `"prototype"` для `F`. Это ещё не «прототип объекта», а обычное свойство `F` с таким именем.

```JS
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal
alert( rabbit.eats ); // true
```

__Установка `Rabbit.prototype = animal` буквально говорит интерпретатору следующее: "При создании объекта через `new Rabbit()` запиши ему `animal` в `[[Prototype]]`".__

![[Pasted image 20240910164251.png]]
На изображении: `"prototype"` – горизонтальная стрелка, обозначающая обычное свойство для `"F"`, а `[[Prototype]]` – вертикальная, обозначающая наследование `rabbit` от `animal`.

## F.prototype по умолчанию и constructor
У каждой функции (за исключением стрелочных) по умолчанию уже есть свойство `"prototype"`.

По умолчанию `"prototype"` – объект с единственным свойством `constructor`, которое ссылается на функцию-конструктор.

```JS
function Rabbit() {}

/* прототип по умолчанию
Rabbit.prototype = { constructor: Rabbit };
*/
```

![[Pasted image 20240910164435.png]]

Мы можем использовать свойство `constructor` существующего объекта для создания нового.

```JS
function Rabbit(name) {
  this.name = name;
  alert(name);
}

let rabbit = new Rabbit("White Rabbit");
let rabbit2 = new rabbit.constructor("Black Rabbit");
```

Это удобно, когда у нас есть объект, но мы не знаем, какой конструктор использовался для его создания (например, он мог быть взят из сторонней библиотеки), а нам необходимо создать ещё один такой объект.

Но, пожалуй, самое важное о свойстве `"constructor"` это то, что…

**…JavaScript сам по себе не гарантирует правильное значение свойства `"constructor"`.**

Да, оно является свойством по умолчанию в `"prototype"` у функций, но что случится с ним позже – зависит только от нас.

В частности, если мы заменим прототип по умолчанию на другой объект, то свойства `"constructor"` в нём не будет.

```JS
function Rabbit() {}

Rabbit.prototype = {
  jumps: true
};

let rabbit = new Rabbit();
alert(rabbit.constructor === Rabbit); // false
```

Таким образом, чтобы сохранить верное свойство `"constructor"`, мы должны добавлять/удалять/изменять свойства у прототипа по умолчанию вместо того, чтобы перезаписывать его целиком:

```JS
function Rabbit() {}
// Не перезаписываем Rabbit.prototype полностью,
// а добавляем к нему свойство
Rabbit.prototype.jumps = true
// Прототип по умолчанию сохраняется, и мы всё ещё имеем доступ к Rabbit.prototype.constructor
```

Или мы можем заново создать свойство `constructor`:
```JS
Rabbit.prototype = {
  jumps: true,
  constructor: Rabbit
};

// теперь свойство constructor снова корректное, так как мы добавили его
```
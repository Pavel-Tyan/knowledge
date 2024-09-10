Для простых объектов доступны следующие методы:
- [Object.keys(obj)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) – возвращает массив ключей.
- [Object.values(obj)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/values) – возвращает массив значений.
- [Object.entries(obj)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) – возвращает массив пар `[ключ, значение]`.

```JS
let user = {
	name: 'John',
	age: 30
};

// Object.keys(user) = ["name", "age"]
// Object.values(user) = ["John", 30]
// Object.entries(user) = [ ["name","John"], ["age",30] ]
```

__Object.keys/values/entries игнорируют символьные свойства__

## `Object.fromEntries`:
Обратное действие метода `entries`. По массиву с ключами и значениями формирует объект.

```JS
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42],
]);

const obj = Object.fromEntries(entries);

console.log(obj);
// Object { foo: "bar", baz: 42 }
```

__Удобно использовать деструктурирующее присваивание в `for..of` для `entries`__
[[Деструктурирующее присваивание]]

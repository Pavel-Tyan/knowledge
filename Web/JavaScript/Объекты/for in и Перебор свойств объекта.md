Для перебора всех свойств объекта используется цикл `for..in`.
```JS
for (key in object) {
	// Тело цикла выполняется для каждого свойства объекта
}
```

```JS
let user = {
	name: 'John',
	age: 30,
	isAdmin: true
};

for (let key in user) {
	//ключи
	alert(key); // name, age, isAdmin
	// значения ключей
	alert(user[key]); // John, 30, true
}
```

[[Порядок в переборе свойств]]
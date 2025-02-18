## Массив зависимостей useEffect

useEffect принимает два параметра. Первый аргумент — это функция обратного вызова, для которой мы будем выполнять побочные эффекты; другой – массив зависимостей. Второй аргумент является необязательным.

Если мы не передадим второй аргумент, побочный эффект в функции обратного вызова будет запускаться снова при каждой визуализации компонента.

```JS
function MyComponent() {  
	useEffect(() => {
		// The side effect will run after every render
	})
}
```

Если мы передаем второй аргумент в виде пустого массива, побочный эффект в функции обратного вызова сработает только один раз при первой визуализации компонента.

```JS
function MyComponent() {  
	useEffect(() => {
		// This side effect will only run once, after the first render
	}, [])
}
```

Если мы передаем свойства или значения состояния во втором аргументе, побочный эффект в функции обратного вызова будет выполняться только при изменении значений свойств или переменной состояния.

```JS
import { useEffect, useState } from 'react'

function MyComponent(props) {
	const [state, setState] = useState('')
	
	useEffect(() => {
		// the side effect will only run when the props or state changed
	}, [prop, state])
}
```
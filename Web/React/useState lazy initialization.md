Предположим, что у нас есть useState:
```js
const initialState = calculateExpensive(props) // Долгие вычисления
const [count, setCount] = React.useState(initialState)
```
При каждом ререндере компонент будет заново вычислять значение и это может сказаться на производительности (UI заблочится до того как initialState посчитается). Проблема в том, что initialState нужно лишь в начале работы приложения.

Решение проблемы:
```js
const getInitialState = () => calculateExpensive(props)
const [count, setCount] = React.useState(getInitialState)
```

Реакт будет вызывать эту функцию только тогда когда ему нужно начальное значение (т.е. 1 раз!!!)

_Важно, что нужно передавать именно через_ `() => {}`

__Любые создаваемые вами переменные или передаваемые вами аргументы создаются при каждом рендеринге.__

```JS
const [count, setCount] = React.useState(calculateExpensive(props))
```

В этом варианте `calculateExpensive(props)` будет каждый раз запускаться при ререндере, даже если нам не нужно начальное значение.


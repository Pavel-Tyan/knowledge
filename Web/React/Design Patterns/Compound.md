В нашем приложении у нас часто есть компоненты, которые принадлежат друг другу. Они зависят друг от друга через общее состояние и совместно используют логику.

Пример

Давайте рассмотрим пример: у нас есть список изображений белок! Помимо простого показа изображений белок, мы хотим добавить кнопку, которая позволит пользователю редактировать или удалять изображение. Мы можем реализовать компонент `FlyOut`, который показывает список, когда пользователь переключает компонент.

![[Pasted image 20250109201645.png]]

В компоненте `FlyOut` у нас по сути есть три вещи: 
* Оболочка `FlyOut`, которая содержит кнопку переключения и список 
* Кнопка `Toggle`, которая переключает список 
* `List`, который содержит список пунктов меню

Используем `Context API`, чтобы удобно делиться данными между компонентами `FlyOut`.

```js
const FlyOutContext = createContext();

function FlyOut(props) {
  const [open, toggle] = useState(false);

  return (
    <FlyOutContext.Provider value={{ open, toggle }}>
      {props.children}
    </FlyOutContext.Provider>
  );
}

function Toggle() {
  const { open, toggle } = useContext(FlyOutContext);

  return (
    <div onClick={() => toggle(!open)}>
      <Icon />
    </div>
  );
}
// Сделаем Toggle свойством FlyOut
FlyOut.Toggle = Toggle;
```

Чтобы фактически предоставить `Toggle` доступ к `FlyOutContext`, нам нужно отобразить его как дочерний компонент `FlyOut`! Мы могли бы просто отобразить это как дочерний компонент. Однако мы также можем сделать компонент `Toggle` свойством компонента `FlyOut`!

__Это означает, что если мы когда-либо захотим использовать весь компонент `FlyOut` (включая `Toggle` и остальное) в любом файле, нам нужно будет только импортировать `FlyOut`!__

```js
import React from "react";
import { FlyOut } from "./FlyOut";

export default function FlyoutMenu() {
  return (
    <FlyOut>
	// Достаточно импортировать только FlyOut
      <FlyOut.Toggle />
    </FlyOut>
  );
}
```

Добавим в итоговый код `List`:
``` js
const FlyOutContext = createContext();

function FlyOut(props) {
  const [open, toggle] = useState(false);

  return (
    <FlyOutContext.Provider value={{ open, toggle }}>
      {props.children}
    </FlyOutContext.Provider>
  );
}

function Toggle() {
  const { open, toggle } = useContext(FlyOutContext);

  return (
    <div onClick={() => toggle(!open)}>
      <Icon />
    </div>
  );
}

function List({ children }) {
  const { open } = useContext(FlyOutContext);
  return open && <ul>{children}</ul>;
}

function Item({ children }) {
  return <li>{children}</li>;
}

FlyOut.Toggle = Toggle;
FlyOut.List = List;
FlyOut.Item = Item;
```

Посмотрим как это можно использовать:

```js
import React from "react";
import { FlyOut } from "./FlyOut";

export default function FlyoutMenu() {
  return (
    <FlyOut>
      <FlyOut.Toggle />
      <FlyOut.List>
        <FlyOut.Item>Edit</FlyOut.Item>
        <FlyOut.Item>Delete</FlyOut.Item>
      </FlyOut.List>
    </FlyOut>
  );
}
```
Теги: #React #Frontend
## Содержание заметки
lazy позволяет отложить загрузку кода компонента до тех пор, пока он не будет отображен в первый раз. Это позволяет не загружать бандл без необходимости. Таким образом мы динамически подгружаем части бандла когда нам это нужно. Это можно использовать прежде всего для разных страничек (роутов).
```TS
import { lazy } from 'react';  

const SomeComponent = lazy(() => import('./SomeComponent.tsx'));
```
Для корректной работы нужно использовать Suspence. Это нужно для того, чтобы вовремя загрузки компонента отображался какой-то контент (например  спиннер).
```TS
<Suspense fallback={<Loading />}>  
	<SomeComponent />  
</Suspense>
```
## Связанные заметки
[[Зачем нужен React Router]]
Теги: #React #React-Router #Frontend
## Содержание заметки
Аутлет (`Outlet` компонент, импортированный из `react-router-dom`) определяет место, где будет отображаться содержание вложенных роутов.
При переходе на конкретный путь сначала загружается лейаут, а затем в аутлет встраивается нужный компонент, соответствующий маршруту.

Дочерние роуты:

```TS
const router = createBrowserRouter([
    {
        path: '/',
        element: <Layout />,
        children: [
            {
                path: '/',
                element: <Menu />,
            },
            {
                path: '/cart',
                element: <Cart />,
            },
        ],
    },
    {
        path: '*',
        element: <Error />,
    },

]);
```

```TS
const Layout = () => {
    return (
        <div>
            <div>Общий контент для всех дочерних роутов</div>
                <Outlet />
        </div>
    );
};
```
## Связанные заметки
[[Link]]
Паттерн **Render Prop** в React используется для создания переиспользуемых компонентов, которые позволяют управлять рендерингом через функцию, переданную как проп.

```JS
const Toggle = ({ render }) => {
  const [isOn, setIsOn] = React.useState(false);

  const toggle = () => setIsOn(!isOn);

  return render({ isOn, toggle });
};

// Использование:
<Toggle render={({ isOn, toggle }) => (
  <button onClick={toggle}>
    {isOn ? 'ON' : 'OFF'}
  </button>
)} />
```

Если компонент должен предоставлять гибкость в том, как он рендерится, использование Render Prop позволяет делегировать этот процесс пользователю компонента.
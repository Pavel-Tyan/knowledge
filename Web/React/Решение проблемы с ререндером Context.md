Теги: #React #Frontend
## Содержание заметки

```TS
const Context = React.createContext(null);

const Component = () => {
    const [theme, setTheme] = useState('default');

    return 
    <Context.Provider value={{theme, setTheme}}>
        <div>
            <Child/>
            // Дочерние компоненты
        </div>
    </Context.Provider>
    <Sibling/>
}
```
Изменение темы будет тригеррить ререндер не только Child, но и Sibling

Вынесем контекст и состояние в отдельный компонент:
```TS
const Context = React.createContext(null);

const Component = () => {
    return (
        <ThemeProvider>
            <div>
                <Child />
            </div>
        </ThemeProvider>
		<Sibling />
    );

};

  

const ThemeProvider = ({ children }) => {
    const [theme, setTheme] = useState('default');

    return (
	    <Context.Provider value={{ theme, setTheme }}>
		    {children}
	    </Context.Provider>;
	)
};
```
Проблема решена. Теперь изменение темы не тригеррит ререндер всего компонента Component. Однако могут быть компоненты, которые лишь изменяют тему, но не используют ее. Они также будут ререндериться при изменении темы.

Создадим второй контекст:
```TS
const Context = React.createContext(null);
const SetterContext = React.createContext(null);

const Component = () => {
    return (
        <ThemeProvider>
            <div>
                <Child />
            </div>
        </ThemeProvider>
		<Sibling />
    );

};

  

const ThemeProvider = ({ children }) => {
    const [theme, setTheme] = useState('default');

    return (
        <Context.Provider value={theme }>
            <SetterContext.Provider value={setTheme}>
	            {children}
            </SetterContext.Provider>
        </Context.Provider>
    );
};
```

Теперь мы можем использовать SetterContext для компонентов, которые меняют тему, но непосредственно не пользуются ею. При изменении темы эти компоненты не будут ререндериться, т.к. они не подписаны на Context с темой, но только на контекст с сеттером. setTheme не будет тригерить ререндер, т.к. мы передаем дочерние компоненты через children, а не напрямую. 

В опр случаях для сеттеров нужно использовать useCallback

Пример с кастомными хуками и useCallback:
```jsx{4-5,8,21,57-58,60-61}
import React, {
  createContext,
  useContext,
  useState,
  useEffect,
  useCallback
} from "react";

// create contexts
const UserContextState = createContext();
const UserContextUpdater = createContext();

// context consumer hook
const useUserContextState = () => {
  // get the context
  const context = useContext(UserContextState);

  // if `undefined`, throw an error
  if (context === undefined) {
    throw new Error("useUserContextState was used outside of its Provider");
  }

  return context;
};

// context consumer hook
const useUserContextUpdater = () => {
  // get the context
  const context = useContext(UserContextUpdater);

  // if `undefined`, throw an error
  if (context === undefined) {
    throw new Error("useUserContextUpdater was used outside of its Provider");
  }

  return context;
};

const UserContextProvider = ({ children }) => {
  // the value that will be given to the context
  const [user, setUser] = useState(null);
  // Every time the user changes, this will be a different function...
  const signout = useCallback(() => {
    setUser(null);
  }, []);

  // fetch a user from a fake backend API
  useEffect(() => {
    const fetchUser = () => {
      // this would usually be your own backend, or localStorage
      // for example
      fetch("https://randomuser.me/api/")
        .then((response) => response.json())
        .then((result) => setUser(result.results[0]))
        .catch((error) => console.log("An error occured"));
    };

    fetchUser();
  }, []);

  return (
    // the Providers gives access to the context to its children
    <UserContextState.Provider value={user}>
      <UserContextUpdater.Provider value={signout}>
        {children}
      </UserContextUpdater.Provider>
    </UserContextState.Provider>
  );
};

export { UserContextProvider, useUserContextState, useUserContextUpdater };
```

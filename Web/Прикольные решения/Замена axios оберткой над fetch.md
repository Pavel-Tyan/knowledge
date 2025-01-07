```JS
function client(endpoint, customConfig) {
	const config = {
		method: 'GET',
		...customConfig,
	}

	return window
		.fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
		.then((response) => response.json())
}
```
Эта клиентская функция позволяет мне совершать вызовы к API моего приложения следующим образом:
```JS
client(`books?query=${encodeURIComponent(query)}`).then(
	(data) => {
		console.log('here are the books', data.books)
	},
	(error) => {
		console.error('oh no, an error happened', error)
	},
)
```
Однако встроенный API window.fetch не обрабатывает ошибки так же, как axios. По умолчанию window.fetch отклоняет promise только в том случае, если сам запрос не выполнен (сетевая ошибка), а не в случае возврата "Ответа об ошибке клиента". К счастью, у объекта Response есть свойство ok, которое мы можем использовать для отклонения обещания в нашей обертке:
```JS
function client(endpoint, customConfig = {}) {
	const config = {
		method: 'GET',
		...customConfig,
	}

	return window
		.fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
		.then(async (response) => {
			if (response.ok) {
				return await response.json()
			} else {
				const errorMessage = await response.text()
				return Promise.reject(new Error(errorMessage))
			}
		})
}
```
Отлично, теперь наша цепочка промисов будет отклоняться, если ответ не в порядке. Следующее, что мы хотим сделать, это иметь возможность отправлять данные на бэкэнд. Мы можем сделать это с помощью нашего текущего API, но давайте упростим это:
```JS
function client(endpoint, { body, ...customConfig } = {}) {
	const headers = { 'Content-Type': 'application/json' }
	const config = {
		method: body ? 'POST' : 'GET',
		...customConfig,
		headers: {
			...headers,
			...customConfig.headers,
		},
	}
	if (body) {
		config.body = JSON.stringify(body)
	}

	return window
		.fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
		.then(async (response) => {
			if (response.ok) {
				return await response.json()
			} else {
				const errorMessage = await response.text()
				return Promise.reject(new Error(errorMessage))
			}
		})
}
```
Отлично, теперь мы можем делать такие вещи:
```JS
client('login', { body: { username, password } }).then(
	(data) => {
		console.log('here the logged in user data', data)
	},
	(error) => {
		console.error('oh no, login failed', error)
	},
)
```
Далее мы хотим иметь возможность делать аутентифицированные запросы. Для этого есть разные подходы, но вот как я это делаю в приложении книжной полки:
```JS
const localStorageKey = '__bookshelf_token__'

function client(endpoint, { body, ...customConfig } = {}) {
	const token = window.localStorage.getItem(localStorageKey)
	const headers = { 'Content-Type': 'application/json' }
	if (token) {
		headers.Authorization = `Bearer ${token}`
	}
	const config = {
		method: body ? 'POST' : 'GET',
		...customConfig,
		headers: {
			...headers,
			...customConfig.headers,
		},
	}
	if (body) {
		config.body = JSON.stringify(body)
	}

	return window
		.fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
		.then(async (response) => {
			if (response.ok) {
				return await response.json()
			} else {
				const errorMessage = await response.text()
				return Promise.reject(new Error(errorMessage))
			}
		})
}
```
Итак, если у нас есть токен в localStorage по этому ключу, то мы добавляем заголовок Authorization (согласно спецификации JWT), который наш сервер может затем использовать для определения того, авторизован ли пользователь. Очень распространенная практика. Еще одна удобная вещь, которую мы можем сделать, — если response.status равен 401, это означает, что токен пользователя недействителен (возможно, истек срок его действия или что-то в этом роде), поэтому мы можем автоматически выйти из системы пользователя и обновить для него страницу:
```JS
const localStorageKey = '__bookshelf_token__'

function client(endpoint, { body, ...customConfig } = {}) {
	const token = window.localStorage.getItem(localStorageKey)
	const headers = { 'content-type': 'application/json' }
	if (token) {
		headers.Authorization = `Bearer ${token}`
	}
	const config = {
		method: body ? 'POST' : 'GET',
		...customConfig,
		headers: {
			...headers,
			...customConfig.headers,
		},
	}
	if (body) {
		config.body = JSON.stringify(body)
	}

	return window
		.fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
		.then(async (response) => {
			if (response.status === 401) {
				logout()
				window.location.assign(window.location)
				return
			}
			if (response.ok) {
				return await response.json()
			} else {
				const errorMessage = await response.text()
				return Promise.reject(new Error(errorMessage))
			}
		})
}

function logout() {
	window.localStorage.removeItem(localStorageKey)
}
```
# Другой пример

```javascript
export const fetchWrapper = {
    get,
    post,
    put,
    delete: _delete
};

function get(url) {
    const requestOptions = {
        method: 'GET'
    };
    return fetch(url, requestOptions).then(handleResponse);
}

function post(url, body) {
    const requestOptions = {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(body)
    };
    return fetch(url, requestOptions).then(handleResponse);
}

function put(url, body) {
    const requestOptions = {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(body)
    };
    return fetch(url, requestOptions).then(handleResponse);    
}

// prefixed with underscored because delete is a reserved word in javascript
function _delete(url) {
    const requestOptions = {
        method: 'DELETE'
    };
    return fetch(url, requestOptions).then(handleResponse);
}

// helper functions

function handleResponse(response) {
    return response.text().then(text => {
        const data = text && JSON.parse(text);
        
        if (!response.ok) {
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
        }

        return data;
    });
}
```
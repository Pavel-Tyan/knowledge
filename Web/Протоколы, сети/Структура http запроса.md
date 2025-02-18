### **1. Стартовая строка (Request Line)**

Определяет цель запроса и содержит три элемента:

- **Метод:** Указывает, что хочет сделать клиент (например, `GET`, `POST`, `PUT`, `DELETE` и др.).
- **URL/Путь:** Указывает, к какому ресурсу клиент обращается на сервере.
- **Версия протокола:** Указывает версию HTTP (`HTTP/1.1`, `HTTP/2`).

**Пример:**

```http
GET /index.html HTTP/1.1
```

---

### **2. Заголовки (Headers)**

Содержат метаинформацию о запросе. Каждый заголовок состоит из имени и значения, разделенных двоеточием.

**Примеры часто используемых заголовков:**

- `Host`: Указывает домен сервера (`example.com`).
- `User-Agent`: Информация о клиенте (браузер, операционная система).
- `Accept`: Указывает, какие форматы данных клиент готов принять (например, `text/html`, `application/json`).
- `Authorization`: Токен или другой метод аутентификации.
- `Content-Type`: Тип данных, передаваемых в теле запроса (`application/json`, `text/plain`).

```http
Host: www.example.com 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) 
Accept: text/html,application/xhtml+xml
```

---

### **3. Тело запроса (Request Body)**

Используется в методах, которые передают данные на сервер (например, `POST`, `PUT`). Может содержать данные в различных форматах, например:

- JSON (`application/json`).
- XML (`application/xml`).
- Форма с URL-кодированием (`application/x-www-form-urlencoded`).
- Файлы в бинарном формате (используя `multipart/form-data`).

```json
{ 
	"username": "john_doe", 
	"password": "12345" 
}
```

Пример полного HTTP-запроса:

```http
POST /login HTTP/1.1 
Host: www.example.com 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) 
Content-Type: application/json 
Content-Length: 39 

{ 
	"username": "john_doe", 
	"password": "12345" 
}
```

[[OSI]]
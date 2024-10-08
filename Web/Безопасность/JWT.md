Теги: #JavaScript #Безопасность #Frontend
## Содержание заметки
#### Что такое JWT?

- **Определение**: JWT (JSON Web Token) — это стандарт, позволяющий надежно передавать информацию между участниками (например, между клиентом и сервером) в виде компактного, самодостаточного JSON объекта.
- **Использование**: Главным образом используется для аутентификации и обмена информацией.

#### Как работает JWT?

- **Компоненты**: JWT состоит из трех частей: Header, Payload, Signature.
    - **Header** содержит информацию о типе токена и используемом алгоритме шифрования.
    - **Payload** содержит набор данных (claims), таких как права доступа пользователя, идентификатор пользователя и т. п.
    - **Signature** создается на основе Header, Payload и секретного ключа, что позволяет верифицировать токен.

#### Процесс работы с JWT

1. **Аутентификация пользователя**: Пользователь отправляет свои учетные данные на сервер.
2. **Выдача токена**: При успешной аутентификации сервер создает JWT токен и отправляет его пользователю.
3. **Использование токена**: Пользователь отправляет JWT в каждом запросе, позволяя серверу верифицировать его (на клиенте зашифрованный токен, расшифровка на происходит на бэке) и предоставлять доступ к запрашиваемым ресурсам.
4. **Обновление токена**: Для повышения безопасности используется механизм Refresh token, который позволяет обновлять JWT по истечении его срока действия.

![[Pasted image 20240621234005.png]]

#### Безопасность JWT

- JWT не хранится на клиенте, что затрудняет его кражу, но в случае утечки токен может быть использован злоумышленниками.
- Введение Refresh Token улучшает безопасность, ограничивая время действия Access Token и предотвращая долгосрочное использование утекших токенов.
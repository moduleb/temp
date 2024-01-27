![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray)
![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

# Работа с токенами

* [Регистрация нового пользователя](#register)
* [Получение access-токена](#get-access_token)
* [Проверка access-токена](#check-access_token)
* [Использование access-токена](#use-access_token)
* [Использование refresh-токена](#use-refresh_token)



<a name="register"></a>
## Регистрация нового пользователя

Для создания нового пользователя необходимо сделать запрос `POST http://127.0.0.1:8000/v1/creat_autor`

#### Запрос

```json
{
  "login": "string",
  "password": "string",
  "email": "user@example.com"
}
```

#### Ответ

Получаем учетные данные зарегестрированного лользоавтеля.

```json
{
  "login": "string",
  "password": "pbkdf2_sha256$720000$7mIMfGeEUYHZVAZrO6vkOh$Jx/pM7ZBYzrLF3AuUAFRztl30BvVnOeh5xoazC8Ir+0=",
  "email": "user@example.com"
}
```

#### Ошибки

* `400 Bad Request` – ошибка в параметрах запроса.

---

<a name="get-access_token"></a>
## Получение access-токена

Для получения access-токена необходимо сделать запрос `POST http://127.0.0.1:8000/token`

#### Запрос

Указываем в запросе учетные данные уже [зарегестрированного](#register) пользователя.

```json
{
  "email": "user@example.com",
  "password": "string"
}
```

#### Ответ

Получаем [access-токен](#use-access_token) и [refresh-токен](#use-refresh_token).

```json
{
  "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTcwNjQyNjQ2NiwiaWF0IjoxNzA2MzQwMDY2LCJqdGkiOiJiYzZhYTAyNzQ1YmE0ZjczODFkOGY4MzNmOWZmMzUwYiIsInVzZXJfaWQiOjJ9.1qGtt-ial3F5vH2nmOZhwk2DQcvZPWwxyW7IceMbo20",
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzA2MzQwMzY2LCJpYXQiOjE3MDYzNDAwNjYsImp0aSI6IjAzNmRhN2VkNTVjNTQ0YmU5MGY2ODhjYjcxM2FhOGVhIiwidXNlcl9pZCI6Mn0.QYt_JOm20yXTP7bfpzdbMGn3ddsYzPgOaLDx_34p7nE"
}
```

#### Ошибки

* `400 Bad Request` – ошибка в параметрах запроса.
* `401 Unauthorized` - Пользотель с такмми учетными данными не зарегестрирован.

---

<a name="check-access_token"></a>

## Проверка access-токена


```
`POST http://127.0.0.1:8000/token/verify`
```

### Запрос

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzA2MzQxMDQxLCJpYXQiOjE3MDYzNDA3NDEsImp0aSI6ImJiZjc4ZGQxMGI2NzQ5NTlhZjY1MzM1ZjZmNDBjNWIzIiwidXNlcl9pZCI6Mn0.pird2eYS2VCfeycWJMFFdVOIgDrTHRyF2CJQbnmY6mA"
}
```

### Ответ

Если токен прошел проверку придет пустой JSON со статус кодом `200`  


#### Ошибки

* `400 Bad Request` – ошибка в параметрах запроса.
* `401 Unauthorized` - недействительный токен.
---

<a name="use-access_token"></a>
## Использование access-токена

Необходимо использовать полученный `access_token` для авторизации,
передавая его в заголовке в формате:

```Authorization: ACCESS_TOKEN```

---

<a name="use-refresh_token"></a>
## Обновление пары access и refresh токенов
`access_token`имеет срок жизни и при его истечении необходимо сделать запрос с `refresh_token` для получения
нового.

```
`POST http://127.0.0.1:8000/token/refresh`
```

### Запрос

```json
{
  "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzA2MzQxMDQxLCJpYXQiOjE3MDYzNDA3NDEsImp0aSI6ImJiZjc4ZGQxMGI2NzQ5NTlhZjY1MzM1ZjZmNDBjNWIzIiwidXNlcl9pZCI6Mn0.pird2eYS2VCfeycWJMFFdVOIgDrTHRyF2CJQbnmY6mA"
}
```

### Ответ

Ответ будет идентичен ответу на получение токенов в [первый раз](#get-access_token):

```json
{
  "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTcwNjQyNjQ2NiwiaWF0IjoxNzA2MzQwMDY2LCJqdGkiOiJiYzZhYTAyNzQ1YmE0ZjczODFkOGY4MzNmOWZmMzUwYiIsInVzZXJfaWQiOjJ9.1qGtt-ial3F5vH2nmOZhwk2DQcvZPWwxyW7IceMbo20",
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzA2MzQwMzY2LCJpYXQiOjE3MDYzNDAwNjYsImp0aSI6IjAzNmRhN2VkNTVjNTQ0YmU5MGY2ODhjYjcxM2FhOGVhIiwidXNlcl9pZCI6Mn0.QYt_JOm20yXTP7bfpzdbMGn3ddsYzPgOaLDx_34p7nE"
}
```

`refresh_token` можно использовать только один раз и только по истечению
срока действия `access_token`.

После получения новой пары access и refresh токенов, их необходимо использовать
в дальнейших запросах в api и запросах на продление токена.


#### Ошибки

* `400 Bad Request` – ошибка в параметрах запроса.
* `401 Unauthorized` - недействительный токен.


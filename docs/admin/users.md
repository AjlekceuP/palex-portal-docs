# Admin Users API

## Базовый URL

`/api/v1/admin/users`

Все эндпоинты требуют:

-   Заголовок `Authorization: Bearer <token>`
-   Роль пользователя: `ADMIN`

------------------------------------------------------------------------

# 1. Получить список пользователей

## `GET /api/v1/admin/users`

Возвращает список пользователей с поддержкой фильтрации, поиска, сортировки и пагинации.

### Query-параметры

| Параметр | Тип | Описание |
|--------|-----|----------|
| `page` | number | Номер страницы (по умолчанию: 1) |
| `limit` | number | Количество записей (по умолчанию: 20, максимум: 100) |
| `sort` | string | Сортировка. Формат: `field:asc` или `field:desc` |
| `search` | string | Глобальный поиск по email, имени и компании |

### Фильтры

| Параметр | Тип | Описание |
|--------|-----|----------|
| `email` | string | Поиск по email (частичное совпадение) |
| `firstName` | string | Поиск по имени |
| `lastName` | string | Поиск по фамилии |
| `company` | string | Поиск по компании |
| `department` | string | Поиск по отделу |
| `post` | string | Поиск по должности |
| `phone` | string | Поиск по телефону |
| `language` | string | Фильтр по языку (`en`, `ru`, `de`) |
| `role` | string | Фильтр по роли (`CLIENT`, `ADMIN`) |
| `groupId` | uuid | Фильтр по группе пользователя |
| `isDeleted` | boolean | Фильтр по soft delete |
| `isBlocked` | boolean | Фильтр по блокировке |

---

## Пример запроса

### Пагинация
```http
GET /api/v1/admin/users?page=1&limit=10  
```
### Поиск
```http
GET /api/v1/admin/users?search=ivan
```
### Фильтр по email
```http
GET /api/v1/admin/users?email=gmail
```
### Фильтр по роли
```http
GET /api/v1/admin/users?role=CLIENT
```

### Пользователи определённой группы
```http
GET /api/v1/admin/users?groupId=uuid
```

### Комбинированный фильтр
```http
GET /api/v1/admin/users?role=CLIENT&company=palex
```

## Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "items": [
            {
                "id": "d56a8d00-d988-4a9a-9c04-e8cd201c79ce",
                "firstName": "Test",
                "lastName": "Client",
                "company": "Palex",
                "department": "CS",
                "post": null,
                "email": "klimova@palexgroup.com",
                "phone": "34134123412",
                "isBlocked": false,
                "role": "CLIENT",
                "language": "en",
                "groupId": null,
                "isDeleted": false,
                "createdAt": "2021-07-06T04:33:32.589Z",
                "updatedAt": "2024-11-22T07:17:31.931Z",
                "deletedAt": null,
                "lastLoginAt": null,
                "emailNotifications": true
            },
            {
                "id": "d13eac14-8c38-4486-869e-f3007493dedd",
                "firstName": "Aleksander",
                "lastName": "Gilev",
                "company": "Palex Soft",
                "department": "Verifika",
                "post": null,
                "email": "support@e-verifika.com",
                "phone": "+79034567890",
                "isBlocked": false,
                "role": "CLIENT",
                "language": "en",
                "groupId": null,
                "isDeleted": false,
                "createdAt": "2019-07-10T01:08:03.534Z",
                "updatedAt": "2020-08-18T07:31:04.109Z",
                "deletedAt": null,
                "lastLoginAt": null,
                "emailNotifications": true
            }
        ],
        "meta": {
            "page": 1,
            "limit": 20,
            "total": 2,
            "pages": 1
        }
    }
}
```

---

# 2. Получить пользователя по ID

## `GET /api/v1/admin/users/:id`

## Пример запроса
```http
GET /api/v1/admin/users/cac9dc11-f795-4cf6-841b-c77d9641231f
```

### Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "cac9dc11-f795-4cf6-841b-c77d9641231f",
        "firstName": "Irina",
        "lastName": "Kovalenko",
        "company": "",
        "department": "",
        "post": null,
        "email": "andrianova@palexgroup.com",
        "phone": "",
        "isBlocked": false,
        "role": "CLIENT",
        "language": "en",
        "groupId": null,
        "isDeleted": false,
        "createdAt": "2019-05-20T11:21:14.323Z",
        "updatedAt": "2025-11-19T06:58:43.724Z",
        "deletedAt": null,
        "lastLoginAt": null,
        "emailNotifications": true
    }
}
```
---

# 3. Обновить пользователя

## `PATCH /api/v1/admin/users/:id`

## Особенности

* Поддерживается частичное обновление.
* Можно передать любое количество полей.
* Необходимо передать хотя бы одно поле.

## Тело запроса (JSON)

Можно передать **любое количество полей** из списка ниже.

| Поле | Тип | Описание                              |
|-----|----|---------------------------------------|
| `firstName` | string | Имя клиента                           |
| `lastName` | string | Фамилия клиента                       |
| `company` | string | Название компании клиента                              |
| `department` | string | Название отдела клиента в компании                                 |
| `email` | string | Адрес электронной почты клиента                                 |
| `post` | string | Название должности клиента в компании                             |
| `phone` | string | Телефон клиента                               |
| `language` | enum | Язык (`en`, `ru`, `de`)               |
| `role` | enum | Роль пользователя (`CLIENT`, `ADMIN`) |
| `groupId` | uuid / null | ID группы                             |
| `isBlocked` | boolean | Блокировка пользователя               |

---

## Пример запроса

```http
PATCH /api/v1/admin/users/cac9dc11-f795-4cf6-841b-c77d9641231f  
```
### Тело запроса

``` json
{
  "firstName": "Ivan",
  "lastName": "Ivanov",
  "company": "Palex"
}
```

**Ответ:** содержит объект пользователя в формате, описанном в разделе **«Получить пользователя по ID»**.

---

# 4. Удалить пользователя (Soft Delete)

## `DELETE /api/v1/admin/users/:id`

## Особенности
* Выполняет **soft delete** пользователя.  
* Удалённые пользователи могут быть восстановлены.  

## Пример запроса
```http
DELETE /api/v1/admin/users/cac9dc11-f795-4cf6-841b-c77d9641231f
```

**Ответ:** содержит объект пользователя в формате, описанном в разделе **«Получить пользователя по ID»**.

---

# 5. Восстановить пользователя

## `POST /api/v1/admin/users/:id/restore`

Восстанавливает ранее удалённого пользователя.

## Пример запроса
```http
POST /api/v1/admin/users/cac9dc11-f795-4cf6-841b-c77d9641231f
```

**Ответ:** содержит объект пользователя в формате, описанном в разделе **«Получить пользователя по ID»**.

------------------------------------------------------------------------

# 6. Изменить пароль пользователя

## `POST /api/v1/admin/users/:id/password`

Изменяет пароль пользователя.  

## Особенности
* Все активные сессии пользователя завершаются.
* Пользователю необходимо войти в систему повторно.

## Пример запроса
```http
POST /api/v1/admin/users/cac9dc11-f795-4cf6-841b-c77d9641231f/password
```

### Тело запроса

``` json
{
  "password": "NewPasswod1"
}
```

### Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": true
}
```

## Требования к паролю

Пароль должен соответствовать следующим правилам:

- минимум **8 символов**
- минимум **одна заглавная буква**
- минимум **одна цифра или специальный символ**
- **без пробелов**
------------------------------------------------------------------------

# 7. Создание пользователя

## `POST /api/v1/admin/users`

Создаёт нового пользователя и отправляет ему **приглашение (invite)** на установку пароля.

## Body параметры

| Поле | Тип | Обязательное | Описание                                               |
|-----|-----|-----|--------------------------------------------------------|
| `firstName` | string | ✔ | Имя                                                    |
| `lastName` | string | ✔ | Фамилия                                                |
| `company` | string | ✔ | Компания                                               |
| `department` | string | ✖ | Отдел                                                  |
| `post` | string | ✖ | Должность                                              |
| `email` | string | ✔ | Email пользователя                                     |
| `phone` | string | ✔ | Телефон                                                |
| `language` | string | ✖ | Язык интерфейса (`en`, `ru`, `de`), по умолчаниею `en` |
| `role` | string | ✖ | Роль (`CLIENT`, `ADMIN`), по умолчанию `CLIENT`        |
| `groupId` | uuid | ✖ | ID группы, по умолчанию `null`                         |
| `isBlocked` | boolean | ✖ | Заблокирован ли пользователь, по умолчанию `false`     |

---

## Пример запроса

```http
POST /api/v1/api/v1/admin/users
```

### Тело запроса

``` json
{
  "firstName": "Ivan",
  "lastName": "Ivanov",
  "company": "Palex",
  "department": "IT",
  "email": "ivan@example.com",
  "phone": "999999999"
}
```

**Ответ:** содержит объект пользователя в формате, описанном в разделе **«Получить пользователя по ID»**.

# 8. Повторная отправка приглашения

## `POST /api/v1/admin/users/:id/resend-invite`

Отправляет пользователю **новое приглашение (invite)** для установки пароля.  

Используется если пользователь:

- не получил письмо
- случайно удалил письмо
- срок действия токена истёк

При повторной отправке:

- старые **неиспользованные invite токены удаляются**
- создаётся новый invite токен
- пользователю отправляется новое письмо

## Пример запроса

```http
POST /api/v1/admin/users/050c212d-0130-46ee-ab59-ef382a49da38/resend-invite
```

### Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": true
}
```
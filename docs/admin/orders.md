# Admin Orders API

## Базовый URL

`/api/v1/admin/orders`

Все эндпоинты требуют:

- Заголовок `Authorization: Bearer <token>`
- Роль пользователя: `ADMIN`

---

# 1. Получить список заказов

## `GET /api/v1/admin/orders`

Возвращает список заказов с поддержкой фильтрации, поиска, сортировки и пагинации.

---

## Query-параметры

| Параметр | Тип | Описание |
|--------|-----|----------|
| `page` | number | Номер страницы (по умолчанию: 1) |
| `limit` | number | Количество записей (по умолчанию: 20, максимум: 100) |
| `sort` | string | Сортировка (поддерживает несколько полей: `field:asc,field2:desc`) |
| `search` | string | Поиск по `productName`, `trackingNumber` |

---

## Фильтры

| Параметр | Тип | Описание        |
|--------|-----|-----------------|
| `userId` | uuid | ID пользователя |
| `statusId` | uuid | ID статуса      |
| `isDeleted` | boolean | Удалён ли заказ |
| `isDraft` | boolean | Черновик        |
| `onHold` | boolean | На паузе        |

> boolean значения поддерживают: `true`, `false`

---

## Примеры запросов

### Пагинация
```http
GET /api/v1/admin/orders?page=1&limit=10
```

### Фильтр
```http
GET /api/v1/admin/orders?userId=d1da2985-90e6-46cf-ae7c-b3bf5b5c02b3&isDraft=false
```

## Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "items": [
            {
                "id": "847db0fb-81c5-461b-b430-561760681076",
                "userId": "d1da2985-90e6-46cf-ae7c-b3bf5b5c02b3",
                "statusId": "408a9ede-f6d0-4fc3-bc24-1ef85d4af94d",
                "trackingNumber": "фывафыва",
                "productName": "asdfasdf",
                "description": "<p>asdf</p>",
                "specialNotes": "<p>asdfasdf</p>",
                "progress": 0,
                "manager": null,
                "preferAt": "2026-04-27T22:00:00.000Z",
                "deadline": null,
                "isDraft": false,
                "isDeleted": false,
                "onHold": false,
                "createdAt": "2026-04-28T09:24:44.827Z",
                "updatedAt": "2026-04-28T09:24:44.828Z",
                "deletedAt": null,
                "draftAt": null,
                "holdAt": null,
                "user": {
                    "id": "d1da2985-90e6-46cf-ae7c-b3bf5b5c02b3",
                    "email": "paa@fin.tsu.ru",
                    "fullName": "Ivan Ivanov"
                },
                "status": {
                    "id": "408a9ede-f6d0-4fc3-bc24-1ef85d4af94d",
                    "name": "New"
                }
            }
        ],
        "meta": {
            "page": 1,
            "limit": 20,
            "total": 1,
            "pages": 1
        }
    }
}
```

---

# 2. Получить заказ по ID

## `GET /api/v1/admin/orders/:id`

## Пример запроса
```http
GET /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076
```

### Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "847db0fb-81c5-461b-b430-561760681076",
        "userId": "d1da2985-90e6-46cf-ae7c-b3bf5b5c02b3",
        "statusId": "408a9ede-f6d0-4fc3-bc24-1ef85d4af94d",
        "trackingNumber": "фывафыва",
        "productName": "asdfasdf",
        "description": "<p>asdf</p>",
        "specialNotes": "<p>asdfasdf</p>",
        "progress": 0,
        "manager": null,
        "preferAt": "2026-04-27T22:00:00.000Z",
        "deadline": null,
        "isDraft": false,
        "isDeleted": false,
        "onHold": false,
        "createdAt": "2026-04-28T09:24:44.827Z",
        "updatedAt": "2026-04-28T09:24:44.828Z",
        "deletedAt": null,
        "draftAt": null,
        "holdAt": null,
        "user": {
            "id": "d1da2985-90e6-46cf-ae7c-b3bf5b5c02b3",
            "email": "paa@fin.tsu.ru",
            "fullName": "Ivan Ivanov"
        },
        "status": {
            "id": "408a9ede-f6d0-4fc3-bc24-1ef85d4af94d",
            "name": "New"
        }
    }
}
```

# 3. Обновить заказ

## `PATCH /api/v1/admin/orders/:id`

Обновляет отдельные поля заказа.

Поддерживается частичное обновление — можно передать одно или несколько полей.

---

## Тело запроса

| Поле       | Тип             | Описание                                        |
| ---------- | --------------- | ----------------------------------------------- |
| `statusId` | uuid            | Новый статус заказа                             |
| `onHold`   | boolean         | Признак постановки заказа на паузу              |
| `deadline` | ISO Date | null | Дедлайн заказа. `null` очищает значение         |
| `manager`  | string | null   | Ответственный менеджер. `null` очищает значение |

> Все поля являются необязательными.   
> Необходимо передать хотя бы одно поле.

---

## Пример запроса

```http
PATCH /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076
```

```json
{
    "statusId": "408a9ede-f6d0-4fc3-bc24-1ef85d4af94d",
    "onHold": true,
    "deadline": "2026-05-15T12:00:00.000Z",
    "manager": "Ivan Petrov"
}
```

---

## Очистка полей

Очистить менеджера:

```json
{
    "manager": null
}
```

Очистить дедлайн:

```json
{
    "deadline": null
}
```

---

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "2852de36-717b-4257-9958-2694494c9943",
    "userId": "a39de45a-6048-4bd9-a4bb-04f3a92770bd",
    "statusId": "26dd7ad1-ad48-47cd-803a-237c3391b2e3",
    "trackingNumber": "ODB005811",
    "productName": "10",
    "description": "<p>k hkjh kjkj</p>",
    "specialNotes": "<p>kj hlkjhlkjhlk</p>",
    "progress": 0,
    "manager": null,
    "preferAt": "2026-06-09T21:00:00.000Z",
    "deadline": null,
    "isDraft": false,
    "isDeleted": false,
    "onHold": true,
    "createdAt": "2026-06-10T09:51:12.954Z",
    "updatedAt": "2026-06-15T07:14:14.618Z",
    "deletedAt": null,
    "draftAt": null,
    "holdAt": null,
    "user": {
      "id": "a39de45a-6048-4bd9-a4bb-04f3a92770bd",
      "email": "g.tentimishov@ulrichmedical.com",
      "fullName": "Galia Tentimishov"
    },
    "status": {
      "id": "26dd7ad1-ad48-47cd-803a-237c3391b2e3",
      "name": "Closed"
    }
  }
}
```



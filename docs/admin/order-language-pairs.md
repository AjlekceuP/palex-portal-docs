# Admin Order Language Pairs API

## Базовый URL

`/api/v1/admin/orders/:orderId/language-pairs`

Все эндпоинты требуют:

* Заголовок `Authorization: Bearer <token>`
* Роль пользователя: `ADMIN`

---

# 1. Получить список языковых пар заказа

## `GET /api/v1/admin/orders/:orderId/language-pairs`

Возвращает список языковых пар для конкретного заказа с поддержкой фильтрации, сортировки и пагинации.

---

## Query-параметры

| Параметр | Тип    | Описание                                             |
| -------- | ------ | ---------------------------------------------------- |
| `page`   | number | Номер страницы (по умолчанию: 1)                     |
| `limit`  | number | Количество записей (по умолчанию: 20, максимум: 100) |
| `sort`   | string | Сортировка (`field:asc` или `field:desc`)            |

---

## Фильтры

| Параметр           | Тип     | Описание               |
| ------------------ | ------- | ---------------------- |
| `sourceLanguageId` | uuid    | ID исходного языка     |
| `targetLanguageId` | uuid    | ID целевого языка      |
| `currentStatusId`  | uuid    | ID текущего статуса    |
| `isDeleted`        | boolean | Фильтр по удалённым    |
| `onHold`           | boolean | Фильтр по hold статусу |

---

## Пример запроса

```http
GET /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs?limit=1
```

---

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "id": "75877e98-7196-49c4-8e62-881303e37509",
        "orderId": "847db0fb-81c5-461b-b430-561760681076",
        "sourceLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
        "targetLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
        "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
        "currentStatusDueTo": null,
        "service": "TRANSLATION",
        "progress": 0,
        "deadline": null,
        "sourceLanguage": {
          "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
          "name": "Adyghe"
        },
        "targetLanguage": {
          "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
          "name": "Adyghe"
        },
        "currentStatus": {
          "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
          "name": "New"
        },
        "isDeleted": false,
        "onHold": false,
        "createdAt": "2026-04-20T09:45:09.885Z",
        "updatedAt": "2026-04-20T09:45:09.885Z",
        "deletedAt": null,
        "holdAt": null
      }
    ],
    "meta": {
      "page": 1,
      "limit": 1,
      "total": 3,
      "pages": 3
    }
  }
}
```

---

# 2. Получить языковую пару

## `GET /api/v1/admin/orders/:orderId/language-pairs/:languagePairId`

---

## Пример запроса

```http
GET /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509
```

---

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "75877e98-7196-49c4-8e62-881303e37509",
    "orderId": "847db0fb-81c5-461b-b430-561760681076",
    "sourceLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
    "targetLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
    "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
    "currentStatusDueTo": null,
    "service": "TRANSLATION",
    "progress": 0,
    "deadline": null,
    "sourceLanguage": {
      "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
      "name": "Adyghe"
    },
    "targetLanguage": {
      "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
      "name": "Adyghe"
    },
    "currentStatus": {
      "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
      "name": "New"
    },
    "isDeleted": false,
    "onHold": false,
    "createdAt": "2026-04-20T09:45:09.885Z",
    "updatedAt": "2026-04-20T09:45:09.885Z",
    "deletedAt": null,
    "holdAt": null
  }
}
```

---

# 3. Создать языковую пару

## `POST /api/v1/admin/orders/:orderId/language-pairs`

---

## Пример запроса

```http
POST /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/
```

## Тело запроса

```json
{
  "sourceLanguageId": "929a8af6-320d-48c4-b21e-1ca4088d6f5d",
  "targetLanguageId": "3ef7b6bd-fcd8-49bc-984e-5bbc725347cf",
  "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
  "service": "TRANSLATION"
}
```

### Пример ответа
```json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "5c6ddaa8-070e-4a81-8727-22aafb436f22",
        "orderId": "847db0fb-81c5-461b-b430-561760681076",
        "sourceLanguageId": "929a8af6-320d-48c4-b21e-1ca4088d6f5d",
        "targetLanguageId": "3ef7b6bd-fcd8-49bc-984e-5bbc725347cf",
        "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
        "currentStatusDueTo": null,
        "service": "TRANSLATION",
        "progress": 0,
        "deadline": null,
        "sourceLanguage": {
            "id": "929a8af6-320d-48c4-b21e-1ca4088d6f5d",
            "name": "Basque"
        },
        "targetLanguage": {
            "id": "3ef7b6bd-fcd8-49bc-984e-5bbc725347cf",
            "name": "French (Swiss)"
        },
        "currentStatus": {
            "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
            "name": "New"
        },
        "isDeleted": false,
        "onHold": false,
        "createdAt": "2026-05-04T08:50:52.431Z",
        "updatedAt": "2026-05-04T08:50:52.431Z",
        "deletedAt": null,
        "holdAt": null
    }
}
```

---

## Особенности

* Нельзя создать дубликат пары (source + target)
* Если пара была удалена → необходимо использовать restore

---

# 4. Обновить языковую пару

## `PATCH /api/v1/admin/orders/:orderId/language-pairs/:languagePairId`

Поддерживает частичное обновление.

---

## Пример запроса

```http
PATCH /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509
```

## Тело запроса

```json
{
  "targetLanguageId": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
  "service": "OTHER"
}
```

---

## Особенности

* Нельзя менять исходный язык

---

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "75877e98-7196-49c4-8e62-881303e37509",
    "orderId": "847db0fb-81c5-461b-b430-561760681076",
    "sourceLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
    "targetLanguageId": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
    "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
    "currentStatusDueTo": null,
    "service": "OTHER",
    "progress": 0,
    "deadline": null,
    "sourceLanguage": {
      "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
      "name": "Adyghe"
    },
    "targetLanguage": {
      "id": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
      "name": "Lao"
    },
    "currentStatus": {
      "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
      "name": "New"
    },
    "isDeleted": false,
    "onHold": false,
    "createdAt": "2026-04-20T09:45:09.885Z",
    "updatedAt": "2026-05-04T09:23:50.935Z",
    "deletedAt": null,
    "holdAt": null
  }
}
```

---

# 5. Удалить языковую пару (Soft Delete)

## `DELETE /api/v1/admin/orders/:orderId/language-pairs/:languagePairId`

---

## Особенности

* Пара не удаляется физически
* Устанавливается `isDeleted = true`

---

## Пример запроса

```http
DELETE /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "75877e98-7196-49c4-8e62-881303e37509",
    "orderId": "847db0fb-81c5-461b-b430-561760681076",
    "sourceLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
    "targetLanguageId": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
    "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
    "currentStatusDueTo": null,
    "service": "OTHER",
    "progress": 0,
    "deadline": null,
    "sourceLanguage": {
      "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
      "name": "Adyghe"
    },
    "targetLanguage": {
      "id": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
      "name": "Lao"
    },
    "currentStatus": {
      "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
      "name": "New"
    },
    "isDeleted": true,
    "onHold": false,
    "createdAt": "2026-04-20T09:45:09.885Z",
    "updatedAt": "2026-05-04T09:26:52.534Z",
    "deletedAt": "2026-05-04T09:26:52.533Z",
    "holdAt": null
  }
}
```

---

# 6. Восстановить языковую пару

## `POST /api/v1/admin/orders/:orderId/:languagePairId/restore`

---

## Особенности

* Восстанавливает soft-deleted запись
* Нельзя восстановить, если уже существует активная такая же пара

---


## Пример запроса

```http
POST /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509/restore
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "75877e98-7196-49c4-8e62-881303e37509",
    "orderId": "847db0fb-81c5-461b-b430-561760681076",
    "sourceLanguageId": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
    "targetLanguageId": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
    "currentStatusId": "d3a5b934-baa2-437b-a59a-5969802d55d8",
    "currentStatusDueTo": null,
    "service": "OTHER",
    "progress": 0,
    "deadline": null,
    "sourceLanguage": {
      "id": "0fe5e797-3181-49c1-ab27-ffd96aff8e69",
      "name": "Adyghe"
    },
    "targetLanguage": {
      "id": "2b4e5909-f4db-468d-b850-9d5a7af5fff5",
      "name": "Lao"
    },
    "currentStatus": {
      "id": "d3a5b934-baa2-437b-a59a-5969802d55d8",
      "name": "New"
    },
    "isDeleted": false,
    "onHold": false,
    "createdAt": "2026-04-20T09:45:09.885Z",
    "updatedAt": "2026-05-04T09:28:00.583Z",
    "deletedAt": null,
    "holdAt": null
  }
}
```
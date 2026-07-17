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
| ------------------ |---------| ---------------------- |
| `sourceLanguageId` | uuid    | ID исходного языка     |
| `targetLanguageId` | uuid    | ID целевого языка      |
| `currentStatusId`  | uuid    | ID текущего статуса    |
| `isDeleted`        | boolean | Фильтр по удалённым    |
| `onHold`           | boolean | Фильтр по hold статусу |
| `service`           | string  | 'TRANSLATION', 'TRANSLATION_PROOFREADING', 'OTHER' |

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

**Ответ:** содержит объект языковой пары в формате, описанном в разделе **«Получить языковую пару»**.

## Особенности

* Нельзя создать дубликат пары (source + target + service)
* Если языковая пара была ранее удалена (soft delete), для ее восстановления необходимо использовать метод `restore`.

---

# 4. Обновить языковую пару

## `PATCH /api/v1/admin/orders/:orderId/language-pairs/:languagePairId`

Поддерживает частичное обновление.

## Пример запроса

```http
PATCH /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509
```

## Тело запроса

```json
{
  "targetLanguageId": "2b4e5909-f4db-468d-b850-9d5a7af5fff5"
}
```

**Ответ:** содержит объект языковой пары в формате, описанном в разделе **«Получить языковую пару»**.

## Особенности

* Нельзя менять исходный язык и сервис.
* Перед изменением проверяется уникальность комбинации `(source + service + target)`. Если такая языковая пара уже существует в заказе, будет возвращена ошибка.

---

# 5. Массовое обновление языковых пар

## `PATCH /api/v1/admin/orders/:orderId/language-pairs/bulk`  

Позволяет за один запрос обновить несколько языковых пар заказа.

Для каждой языковой пары можно изменить:

- текущий статус (`currentStatusId`);
- дату выполнения (`deadline`);
- признак паузы (`onHold`).

Обновление выполняется в рамках одной транзакции.

## Пример запроса

```http
PATCH /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/bulk
```

## Тело запроса

```json
{
  "items": [
    {
      "id": "9f5f19ca-37a5-4b98-bfad-1f54bfc787fb",
      "currentStatusId": "cb2dd5d7-60db-45dc-9d85-4df91b3052de",
      "deadline": "2026-08-01T10:00:00.000Z",
      "onHold": true
    },
    {
      "id": "d3a793ab-57f6-4b93-a36a-4af9cc3dd5d2",
      "onHold": false
    }
  ]
}
```

**Ответ:** содержит массив объектов языковых пар в формате, описанном в разделе **«Получить языковую пару»**.

## Особенности  
* При изменении статуса создается запись в истории статусов.
* При изменении признака `onHold` создаются соответствующие сообщения.
---


# 6. Удалить языковую пару (Soft Delete)

## `DELETE /api/v1/admin/orders/:orderId/language-pairs/:languagePairId`

## Особенности

* Пара не удаляется физически
* Устанавливается `isDeleted = true`

## Пример запроса

```http
DELETE /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509
```

**Ответ:** содержит объект языковой пары в формате, описанном в разделе **«Получить языковую пару»**.

---

# 7. Восстановить языковую пару

## `POST /api/v1/admin/orders/:orderId/language-pairs/:languagePairId/restore`

## Особенности

* Восстанавливает ранее удаленную (soft-deleted) языковую пару.

## Пример запроса

```http
POST /api/v1/admin/orders/847db0fb-81c5-461b-b430-561760681076/language-pairs/75877e98-7196-49c4-8e62-881303e37509/restore
```

**Ответ:** содержит объект языковой пары в формате, описанном в разделе **«Получить языковую пару»**.
# Admin Statuses API

## Базовый URL

`/api/v1/admin/statuses`

Все эндпоинты требуют:

- Заголовок `Authorization: Bearer <token>`
- Роль пользователя: `ADMIN`

---

# 1. Получить список статусов

## `GET /api/v1/admin/statuses`

Возвращает список статусов.

### Query-параметры

| Параметр         | Тип       | Описание                                             |
|------------------|-----------|------------------------------------------------------|
| page             | number    | Номер страницы (по умолчанию: 1)                     |
| limit            | number    | Количество записей (по умолчанию: 20, максимум: 100) |
| sort             | string    | Сортировка `field:asc` или `field:desc`              |
| search           | string    | Поиск по name и slug                                 |

### Фильтры

| Параметр   | Тип     | Описание                          |
|------------|---------|-----------------------------------|
| type       | enum    | Тип статуса (`ORDER`, `ORDER_LANGUAGE`, `QUOTE`)      |
| isArchived | boolean | Фильтр по архивированным статусам |

------------------------------------------------------------------------

## Пример запроса

GET `/api/v1/admin/statuses?page=1&limit=10`

------------------------------------------------------------------------
## Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "items": [
            {
                "id": "0e06d5f4-6bf7-4669-b258-fa427629fc9b",
                "type": "ORDER",
                "name": "Rejected Deadline",
                "slug": "statuses_order.rejected_deadline",
                "position": 0,
                "isArchived": false,
                "createdAt": "2019-10-11T13:35:53.684Z",
                "updatedAt": "2025-12-23T02:42:49.311Z",
                "archivedAt": null
            },
        ],
        "meta": {
            "page": 1,
            "limit": 10,
            "total": 30,
            "pages": 3
        }
    }
}
            
```
------------------------------------------------------------------------

# 2. Получить статус по ID

## `GET /api/v1/admin/statuses/:id`

Возвращает информацию о конкретном статусе.

## Пример запроса

GET `/api/v1/admin/statuses/dcd7c780-4296-4069-8792-1bbebe70baec`

### Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "dcd7c780-4296-4069-8792-1bbebe70baec",
        "type": "QUOTE",
        "name": "Complete",
        "slug": "statuses_quote.complete",
        "position": 3,
        "isArchived": false,
        "createdAt": "2019-05-19T12:44:49.045Z",
        "updatedAt": "2020-06-17T23:15:51.314Z",
        "archivedAt": null
    }
}
```

# 3. Создать статус

## `POST /api/v1/admin/statuses`

### Body параметры

| Поле     | Тип    | Обязательное | Описание         |
|----------|--------|--------------|------------------|
| type     | enum   | ✔            | Тип статуса      |
| name     | string | ✔            | Название статуса |
| position | number | ✖            | Позиция          |

---

## Пример запроса

```http
POST /api/v1/admin/statuses
```

### Тело запроса

``` json
{
  "type": "ORDER",
  "name": "New status"
}
```

### Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "6a55b548-19db-4aad-b637-b76a7f747d68",
        "type": "ORDER",
        "name": "New status",
        "slug": "statuses_order.new_status",
        "position": 1,
        "isArchived": false,
        "createdAt": "2026-03-16T10:07:45.742Z",
        "updatedAt": "2026-03-16T10:07:45.742Z",
        "archivedAt": null
    }
}
```
---

# 4. Обновить статус

## `PATCH /api/v1/admin/statuses/:id`

### Body параметры

| Поле     | Тип    | Описание         |
|----------|--------|------------------|
| name     | string | Название статуса |
| position | number | Позиция статуса  |

## Пример запроса

``` http
PATCH /api/v1/admin/statuses/6a55b548-19db-4aad-b637-b76a7f747d68
 ```

### Тело запроса

``` json
{
  "name": "New status1"
}
```

### Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "6a55b548-19db-4aad-b637-b76a7f747d68",
        "type": "ORDER",
        "name": "New status1",
        "slug": "statuses_order.new_status1",
        "position": 1,
        "isArchived": false,
        "createdAt": "2026-03-16T10:07:45.742Z",
        "updatedAt": "2026-03-16T10:10:29.288Z",
        "archivedAt": null
    }
}
```
---

# 5. Архивировать статус

## `POST /api/v1/admin/statuses/:id/archive`

Переводит статус в архив.

**Ответ:** содержит объект статуса в формате, описанном в разделе **«Получить статус по ID»**.

---

# 6. Восстановить статус

## `POST /api/v1/admin/statuses/:id/restore`

Восстанавливает ранее архивированный статус.

**Ответ:** содержит объект статуса в формате, описанном в разделе **«Получить статус по ID»**.

---

# Типы статусов

| Тип            | Описание             |
|----------------|----------------------|
| ORDER          | Статус заказа        |
| ORDER_LANGUAGE | Статус языковой пары |

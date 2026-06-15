# Admin Languages API

## Базовый URL

`/api/v1/admin/languages`

Все эндпоинты требуют:
- Заголовок `Authorization: Bearer <token>`
- Роль пользователя: `ADMIN`

---

# 1. Получить список языков

## `GET /admin/languages`

Возвращает список языков с пагинацией.

### Query-параметры

| Параметр    | Тип     | Описание                                             |
|-------------|---------|------------------------------------------------------|
| `page`      | number  | Номер страницы (по умолчанию: 1)                     |
| `limit`     | number  | Количество записей (по умолчанию: 20, максимум: 100) |
| `search`    | string  | Поиск по названию группы **(не используется)**       |
| `isDeleted` | boolean | Фильтр по soft delete                                |

------------------------------------------------------------------------

## Пример запроса

GET `/api/v1/admin/languages?page=1&limit=10`

Response:

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "id": "307e9dcc-15f6-43e7-b0b6-33024cc22254",
        "name": "Abkhazian",
        "code": "ckb",
        "slug": "languages.abkhazian",
        "isDeleted": false,
        "createdAt": "2020-06-29T03:44:50.500Z",
        "updatedAt": "2025-08-20T05:34:42.529Z",
        "deletedAt": null
      }
      
    ]
  },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 139,
    "pages": 7
  }
}
```

------------------------------------------------------------------------

# 2. Получить язык по ID

## `GET /admin/languages/:id`

Возвращает один язык

## Пример запроса

GET `/api/v1/admin/languages/aa038655-6a31-4d94-865e-058b7d7b3e68`

### Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "aa038655-6a31-4d94-865e-058b7d7b3e68",
    "name": "Cebuano",
    "code": "ceb",
    "slug": "languages.cebuano",
    "isDeleted": false,
    "createdAt": "2020-06-29T03:44:02.464Z",
    "updatedAt": "2025-05-23T08:23:12.250Z",
    "deletedAt": null,
    "legacyId": 110
  }
}
```
------------------------------------------------------------------------

# 3. Обновить язык

## `PATCH /admin/languages/:id`

### Пример запроса
PATCH `/api/v1/admin/languages/307e9dcc-15f6-43e7-b0b6-33024cc22254`

### Тело запроса

``` json
{
  "name": "German Updated",
  "code": "DEU",
  "isDeleted": true
}
```

### Пример ответа  
``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "307e9dcc-15f6-43e7-b0b6-33024cc22254",
        "name": "German Updated",
        "code": "DEU",
        "slug": "languages.german_updated",
        "isDeleted": true,
        "createdAt": "2020-06-29T03:44:50.500Z",
        "updatedAt": "2026-03-03T10:27:07.454Z",
        "deletedAt": "2026-03-03T10:27:07.452Z",
        "legacyId": 113
    }
}
```

------------------------------------------------------------------------

# 4. Удалить язык (Soft Delete)

## `DELETE /admin/languages/:id`

### Пример запроса
DELETE `/api/v1/admin/languages/03b717a0-ced7-4812-8f3c-c87d74cacbbd`

### Пример ответа
``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "03b717a0-ced7-4812-8f3c-c87d74cacbbd",
        "name": "Chinese (Simplified)",
        "code": "zh-CN",
        "slug": "languages.chinese_simplified",
        "isDeleted": true,
        "createdAt": "2019-05-19T12:44:48.771Z",
        "updatedAt": "2026-03-10T04:05:26.774Z",
        "deletedAt": "2026-03-10T04:05:26.773Z",
        "legacyId": 12
    }
}
```

### Дополнение
1. slug - создается автоматически из поля name   
   Примеры:  
   Chinese (Simplified) → languages.chinese_simplified  
   English → languages.english
2. Поля name/code/slug должны быть уникальны
# Admin Groups API

## Базовый URL

`/api/v1/admin/groups`

Все эндпоинты требуют:

-   Заголовок `Authorization: Bearer <token>`
-   Роль пользователя: `ADMIN`

------------------------------------------------------------------------

# 1. Получить список групп

## `GET /admin/groups`

Возвращает список групп с пагинацией.

### Query-параметры

| Параметр        | Тип     | Описание                                             |
|-----------------|---------|------------------------------------------------------|
| `page`          | number  | Номер страницы (по умолчанию: 1)                     |
| `limit`         | number  | Количество записей (по умолчанию: 20, максимум: 100) |
| `search`        | string  | Поиск по названию группы **(не используется)**       |
| `isDeleted`     | boolean | Фильтр по soft delete                                |
| `includeUsers`  | boolean | Включить список пользователей в ответ                |

------------------------------------------------------------------------

## Пример запроса

GET `/api/v1/admin/groups?page=1&limit=10&includeUsers=true`

------------------------------------------------------------------------

## Пример ответа

``` json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "id": "uuid",
        "name": "Название группы",
        "isDeleted": false,
        "createdAt": "2024-01-01T10:00:00.000Z",
        "deletedAt": null,
        "users": [
          {
            "id": "uuid",
            "email": "user@example.com",
            "fullName": "Иван Иванов"
            "company": "Corwin - Romaguera",
            "department": null            
          }
        ]
      }
    ],
    "meta": {
      "page": 1,
      "limit": 10,
      "total": 5,
      "pages": 1
    }
  }
}
```

------------------------------------------------------------------------

# 2. Получить группу по ID

## `GET /admin/groups/:id`

Возвращает одну группу вместе со списком пользователей.

## Пример запроса

GET `/api/v1/admin/groups/93282efb-2e48-4ef2-9302-4d51f9f53a6a`


### Пример ответа

``` json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "93282efb-2e48-4ef2-9302-4d51f9f53a6a",
    "name": "",
    "isDeleted": false,
    "createdAt": "2024-01-01T10:00:00.000Z",
    "deletedAt": null,
    "users": [
      {
        "id": "uuid",
        "email": "user@example.com",
        "fullName": "Иван Иванов"
        "company": "Corwin - Romaguera",
        "department": null
      }
    ]
  }
}
```

------------------------------------------------------------------------

# 3. Создать группу

## `POST /admin/groups`

### Тело запроса

``` json
{
    "name": "Новая группа"
}
```

### Пример ответа

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "5ca14813-cc53-4e54-a53b-a3fccaa257e4",
        "name": "",
        "isDeleted": false,
        "createdAt": "2026-03-03T07:42:57.720Z",
        "updatedAt": "2026-03-03T07:42:57.720Z",
        "deletedAt": null
    }
}
```

------------------------------------------------------------------------

# 4. Удалить группу (Soft Delete)

## `DELETE /admin/groups/:id`  

!!! Удаление возможно только если в группе нет пользователей.

### Ошибка (если есть пользователи)

``` json
{
    "success": false,
    "message": "Cannot delete group with users",
    "data": {}
}
```

### Успешный ответ

``` json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "37fd70e8-bd06-4c64-9ebc-4bd447bba22f",
        "name": "",
        "isDeleted": true,
        "createdAt": "2026-03-03T07:34:06.177Z",
        "updatedAt": "2026-03-03T07:46:51.367Z",
        "deletedAt": "2026-03-03T07:46:51.365Z"
    }
}
```
# Admin Translations API

## Базовый URL

`/api/v1/admin/translations`

Все эндпоинты требуют:

-   Заголовок `Authorization: Bearer <token>`
-   Роль пользователя: `ADMIN`

Переводы используются для:

-   **Локализации интерфейса фронтенда**
-   **Шаблонов email‑писем**

Через API **нельзя создавать или удалять переводы**.\
Разрешено только **редактирование существующих записей**.

------------------------------------------------------------------------

# 1. Получить список переводов

## `GET /admin/translations`

Возвращает список переводов.

## Query‑параметры

| Параметр         | Тип       | Описание                                             |
|------------------|-----------|------------------------------------------------------|
| page             | number    | Номер страницы (по умолчанию: 1)                     |
| limit            | number    | Количество записей (по умолчанию: 20, максимум: 100) |
| sort             | string    | Сортировка `field:asc` или `field:desc`              |
| search           | string    | Поиск по slug, en, ru, de                         |

## Фильтры

| Параметр | Тип      | Описание                    |
|----------|----------|-----------------------------|
| type     | enum     | INTERFACE, EMAIL            |
| slug     | string   | Фильтр по slug              |
| en       | string   | Поиск по английскому тексту |
| ru       | string   | Поиск по русскому тексту    |
| de       | string   | Поиск по немецкому тексту   |

------------------------------------------------------------------------

## Примеры запросов

### Пагинация
```http
GET /api/v1/admin/translations?page=1&limit=10
```

### Поиск
```http
GET /api/v1/admin/translations?search=login
```

### Фильтр по типу
```http
GET /api/v1/admin/translations?type=INTERFACE
```

### Фильтр по slug
```http
GET /api/v1/admin/translations?slug=email.reset
```

------------------------------------------------------------------------

## Пример ответа

``` json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "id": "2b3d0e16-94e5-4d2b-8c8a-6a8f5d01a6f2",
        "type": "INTERFACE",
        "slug": "login.title",
        "en": "Login",
        "ru": "Вход",
        "de": "Anmeldung",
        "createdAt": "2026-03-10T10:21:32.123Z",
        "updatedAt": "2026-03-10T10:21:32.123Z"
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

------------------------------------------------------------------------

# 2. Получить перевод по ID

## `GET /admin/translations/:id`

Возвращает одну запись перевода.


## Пример запроса
```http
GET /api/v1/admin/translations/2b3d0e16-94e5-4d2b-8c8a-6a8f5d01a6f2
```


## Пример ответа

``` json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "2b3d0e16-94e5-4d2b-8c8a-6a8f5d01a6f2",
    "type": "INTERFACE",
    "slug": "login.title",
    "en": "Login",
    "ru": "Вход",
    "de": "Anmeldung",
    "createdAt": "2026-03-10T10:21:32.123Z",
    "updatedAt": "2026-03-10T10:21:32.123Z"
  }
}
```

------------------------------------------------------------------------

# 3. Обновить перевод

## `PATCH /admin/translations/:id`

Обновляет тексты перевода.

## Тело запроса

| Поле | Тип | Описание                             |
|-----|----|----------------------------------------|
| `en` | string | Английский перевод                |
| `ru` | string | Русский перевод                        |
| `de` | string | Немецкий перевод             |


## Пример запроса
```http
PATCH /api/v1/admin/translations/2b3d0e16-94e5-4d2b-8c8a-6a8f5d01a6f2
```

### Тело запроса

``` json
{
  "en": "Sign in",
  "ru": "Войти"
}
```

## Пример ответа

``` json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "2b3d0e16-94e5-4d2b-8c8a-6a8f5d01a6f2",
    "type": "INTERFACE",
    "slug": "login.title",
    "en": "Sign in",
    "ru": "Войти",
    "de": "Anmeldung",
    "createdAt": "2026-03-10T10:21:32.123Z",
    "updatedAt": "2026-03-16T08:41:10.412Z"
  }
}
```
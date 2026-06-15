# Admin Order Messages API

## Базовый URL

`/api/v1/admin/orders/:orderId/messages`

Все эндпоинты требуют:
- Заголовок `Authorization: Bearer <token>`
- Роль пользователя: `ADMIN`

---

# 1. Получить список сообщений

### `GET /api/v1/admin/orders/:orderId/messages`

### Query параметры

| Параметр   | Тип     | Описание                                     |
|------------|---------|----------------------------------------------|
| page       | number  | Номер страницы                               |
| limit      | number  | Количество записей                           |
| search     | string  | Поиск по тексту и автору                     |
| authorType | string  | Тип автора (`USER`, `INTEGRATION`, `SYSTEM`) |
| isDeleted  | boolean | Фильтр по удалённым                          |


### Пример запроса
```http
GET /api/v1/admin/orders/99d4f19c-b2eb-44ee-9344-495945b9b761/messages
```

### Ответ

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "items": [
      {
        "id": "2742f935-d254-432e-b775-55a531209bc6",
        "orderId": "99d4f19c-b2eb-44ee-9344-495945b9b761",
        "userId": "6529109e-8781-44f6-8300-18d277217b89",
        "text": "Please find attached corrected Word document.",
        "isDeleted": false,
        "createdAt": "2024-10-18T09:19:52.209Z",
        "updatedAt": "2024-10-18T09:19:52.242Z",
        "deletedAt": null,
        "authorType": "USER",
        "externalAuthor": "Galia Tentimishov",
        "files": [
          {
            "id": "f2e31404-a13d-4f46-89a9-b7ba8d29105b",
            "name": "gamaxxd10810-zhenr132024-04word.docx",
            "originalName": "GA_Max_XD10810-ZH_EN_R13_2024-04_Word.docx",
            "storageKey": "uploads/405-961356a7-506b-4793-970f-0174660b862e/GA_Max_XD10810-ZH_EN_R13_2024-04_Word.docx",
            "size": 0,
            "mime": "",
            "isDeleted": false,
            "deletedAt": null,
            "createdAt": "2024-10-18T09:19:52.24"
          }
        ]
      }
    ],
    "meta": {
      "page": 1,
      "limit": 20,
      "total": 3,
      "pages": 1
    }
  }
}
```

---

# 2. Получить сообщение по ID

### GET `/api/v1/admin/orders/:orderId/messages/:messageId`

## Пример

GET `/api/v1/admin/orders/99d4f19c-b2eb-44ee-9344-495945b9b761/messages/2742f935-d254-432e-b775-55a531209bc6`

### Ответ

```json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "2742f935-d254-432e-b775-55a531209bc6",
        "orderId": "99d4f19c-b2eb-44ee-9344-495945b9b761",
        "userId": "6529109e-8781-44f6-8300-18d277217b89",
        "user": {
            "id": "6529109e-8781-44f6-8300-18d277217b89",
            "fullName": "Galia Tentimishov"
        },
        "authorType": "USER",
        "externalAuthor": null,
        "text": "Please find attached corrected Word document.",
        "createdAt": "2024-10-18T09:19:52.209Z",
        "updatedAt": "2024-10-18T09:19:52.242Z",
        "isDeleted": false,
        "deletedAt": null,
        "files": [
            {
                "id": "f2e31404-a13d-4f46-89a9-b7ba8d29105b",
                "name": "gamaxxd10810-zhenr132024-04word.docx",
                "originalName": "GA_Max_XD10810-ZH_EN_R13_2024-04_Word.docx",
                "storageKey": "uploads/405-961356a7-506b-4793-970f-0174660b862e/GA_Max_XD10810-ZH_EN_R13_2024-04_Word.docx",
                "size": 0,
                "mime": "",
                "createdAt": "2024-10-18T09:19:52.240Z"
            }
        ]
    }
}
```

---

# 3. Создать сообщение

### POST `/api/v1/admin/orders/:orderId/messages`

- Если authorType=USER, то externalAuthor пусто, при выводе используется данные userId от которого создается сообщение
- Если authorType=INTEGRATION, необходимо указать externalAuthor
- Если authorType=INTEGRATION можно указать createdAt

## Пример

POST `/api/v1/admin/orders/99d4f19c-b2eb-44ee-9344-495945b9b761/messages/`

### Тело запроса

``` json
{
  "text": "Message with files",
  "authorType": "USER",
  "files": [
    {
      "name": "file1.pdf",
      "originalName": "file1.pdf",
      "storageKey": "test/file1.pdf",
      "size": 123,
      "mime": "application/pdf"
    },
    {
      "name": "file2.txt",
      "originalName": "file2.txt",
      "storageKey": "test/file2.txt"
    }
  ]
}
```

### Ответ

```json
{
    "success": true,
    "message": "OK",
    "data": {
        "id": "feb2dda4-381f-45e4-b29e-609e62791a3b",
        "orderId": "99d4f19c-b2eb-44ee-9344-495945b9b761",
        "userId": "df5e9704-934e-4f8f-ac60-286799c4a714",
        "user": {
            "id": "df5e9704-934e-4f8f-ac60-286799c4a714",
            "fullName": "System Administrator"
        },
        "authorType": "USER",
        "externalAuthor": null,
        "text": "Message with files",
        "createdAt": "2026-03-18T02:10:55.744Z",
        "updatedAt": "2026-03-18T02:10:55.744Z",
        "isDeleted": false,
        "deletedAt": null,
        "files": [
            {
                "id": "7d5bc29c-2b52-4213-982d-237641ba321b",
                "name": "file1.pdf",
                "originalName": "file1.pdf",
                "storageKey": "test/file1.pdf",
                "size": 123,
                "mime": "application/pdf",
                "createdAt": "2026-03-18T02:10:55.756Z"
            },
            {
                "id": "2d6f39d9-06c2-4b67-9a99-d1317f4771e3",
                "name": "file2.txt",
                "originalName": "file2.txt",
                "storageKey": "test/file2.txt",
                "size": null,
                "mime": null,
                "createdAt": "2026-03-18T02:10:55.756Z"
            }
        ]
    }
}
```

---

# 4. Обновить сообщение

### PATCH `/api/v1/admin/orders/:orderId/messages/:messageId`

---

# 5. Удалить сообщение (soft delete)

### DELETE `/api/v1/admin/orders/:orderId/messages/:messageId`

---

# 6. Восстановить сообщение

### POST `/api/v1/admin/orders/:orderId/messages/:messageId/restore`

---

# 7. Добавить файл в сообщение

- Добавляется информация об файле
- Загрузка файла происходит другим методом

### POST `/api/v1/admin/orders/:orderId/messages/:messageId/files`

---

# 8. Удалить файл из сообщения (Soft Delete)

- Файл остается в хранилище и удаляется из него другим способом
- Не отбражается у клиента в сообщениях

###  `DELETE /api/v1/admin/orders/:orderId/messages/:messageId/files/:fileId`

---

# 9. Восстановить удаленный файл сообщения

### `POST /api/v1/admin/orders/:orderId/messages/:messageId/files/:fileId/restore`

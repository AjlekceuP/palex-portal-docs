# Admin Order Files API

## Базовый URL

`/api/v1/admin/orders/:orderId/files`

Все эндпоинты требуют:

* Заголовок `Authorization: Bearer <token>`
* Роль пользователя: `ADMIN`

> **Важно**
>
> Данный API предназначен **только для работы с файлами заказа** (`SOURCE`, `RESULT`, `GENERATED`, `QUOTE`).
>
> Файлы сообщений (`ATTACHMENT`) через этот API **не поддерживаются**.
>
> Для работы с вложениями сообщений используйте API сообщений заказа:
>
> ```
> /api/v1/admin/orders/:orderId/messages/:messageId/files
> ```
---
> **Особенности типа QUOTE**  
> 
> В рамках одного заказа может существовать не более одного активного файла типа QUOTE.
> 
> При создании нового файла QUOTE предыдущий активный файл QUOTE автоматически помечается как удалённый (isDeleted = true).
> 
> При восстановлении (restore) файла QUOTE все остальные активные файлы QUOTE данного заказа также автоматически архивируются.  
---

# 1. Получить список файлов заказа

## `GET /api/v1/admin/orders/:orderId/files`

Возвращает список файлов заказа. Файлы, прикрепленные к сообщениям (ATTACHMENT), в результат не включаются.

---

## Query-параметры

| Параметр | Тип    | Описание                                             |
| -------- | ------ | ---------------------------------------------------- |
| `page`   | number | Номер страницы (по умолчанию: 1)                     |
| `limit`  | number | Количество записей (по умолчанию: 20, максимум: 100) |
| `sort`   | string | Сортировка (`field:asc` или `field:desc`)            |
| `search` | string | Поиск по имени файла                                 |
| `type`   | enum   | Тип файла                                            |

---

## Типы файлов

* `SOURCE` — исходный файл
* `RESULT` — результат
* `GENERATED` — сгенерированный системой
* `QUOTE` — файл расчёта


---

## Пример запроса

```http
GET /api/v1/admin/orders/0a617ff5-6346-45f8-8eb7-19f8a50b770b/files
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
        "id": "2f158839-344c-4c26-93dd-d3cd7ecf2074",
        "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
        "messageId": null,
        "originalName": "ulrich medical_Quote No._20200624_52823.xlsx",
        "type": "QUOTE",
        "name": "ulrich-medicalquote-no2020062452823.xlsx",
        "storageKey": "uploads/188-f9eecf6b-3c3c-4a6a-9d08-30c66d512321/ulrich medical_Quote No._20200624_52823.xlsx",
        "size": 0,
        "mime": "",
        "isDeleted": false,
        "createdAt": "2026-04-03T04:32:32.721Z",
        "deletedAt": null
      },
      {
        "id": "02fdc775-1f5d-4781-9ec7-8db4dc82efdb",
        "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
        "messageId": null,
        "originalName": "XD 10830_DE_GA_Patientenschlauch für ulricheasyINJECT CT_MRT-Injektoren R4_2020-05.pdf",
        "type": "SOURCE",
        "name": "xd-10830degapatientenschlauch-fur-ulricheasyinject-ctmrt-injektoren-r42020-05.pdf",
        "storageKey": "uploads/132-8b2e45c6-aef7-4aec-ab61-8eba69e1c07f/XD 10830_DE_GA_Patientenschlauch für ulricheasyINJECT CT_MRT-Injektoren R4_2020-05.pdf",
        "size": 0,
        "mime": "",
        "isDeleted": false,
        "createdAt": "2020-05-14T11:14:02.594Z",
        "deletedAt": null
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

# 2. Получить файл

## `GET /api/v1/admin/orders/:orderId/files/:fileId`

---

## Пример запроса

```http
GET /api/v1/admin/orders/0a617ff5-6346-45f8-8eb7-19f8a50b770b/files/2f158839-344c-4c26-93dd-d3cd7ecf2074
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "2f158839-344c-4c26-93dd-d3cd7ecf2074",
    "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
    "messageId": null,
    "originalName": "ulrich medical_Quote No._20200624_52823.xlsx",
    "type": "QUOTE",
    "name": "ulrich-medicalquote-no2020062452823.xlsx",
    "storageKey": "uploads/188-f9eecf6b-3c3c-4a6a-9d08-30c66d512321/ulrich medical_Quote No._20200624_52823.xlsx",
    "size": 0,
    "mime": "",
    "isDeleted": false,
    "createdAt": "2026-04-03T04:32:32.721Z",
    "deletedAt": null
  }
}
```

---

# 3. Создать файл

## `POST /api/v1/admin/orders/:orderId/files`

---

## Body параметры

| Поле           | Тип    | Обязательное | Описание                      |
| -------------- | ------ | ------------ | ----------------------------- |
| `type`         | enum   | ✔            | Тип файла                     |
| `name`         | string | ✔            | Имя файла                     |
| `storageKey`   | string | ✔            | Ключ в хранилище              |
| `originalName` | string | ✖            | Оригинальное имя              |
| `size`         | number | ✖            | Размер                        |
| `mime`         | string | ✖            | MIME тип                      |

---

## Пример запроса

```json
{
  "type": "SOURCE",
  "name": "document.pdf",
  "storageKey": "files/doc.pdf"
}
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "243462a3-fd59-4c1b-88ed-b9228ef2a06c",
    "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
    "messageId": null,
    "originalName": null,
    "type": "SOURCE",
    "name": "document.pdf",
    "storageKey": "files/doc.pdf",
    "size": null,
    "mime": null,
    "isDeleted": false,
    "createdAt": "2026-05-04T10:04:33.489Z",
    "deletedAt": null
  }
}
```

---

# 4. Удалить файл (Soft Delete)

## `DELETE /api/v1/admin/orders/:orderId/files/:fileId`

---

## Пример запроса

```http
DELETE /api/v1/admin/orders/0a617ff5-6346-45f8-8eb7-19f8a50b770b/files/2f158839-344c-4c26-93dd-d3cd7ecf2074
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "2f158839-344c-4c26-93dd-d3cd7ecf2074",
    "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
    "messageId": null,
    "originalName": "ulrich medical_Quote No._20200624_52823.xlsx",
    "type": "QUOTE",
    "name": "ulrich-medicalquote-no2020062452823.xlsx",
    "storageKey": "uploads/188-f9eecf6b-3c3c-4a6a-9d08-30c66d512321/ulrich medical_Quote No._20200624_52823.xlsx",
    "size": 0,
    "mime": "",
    "isDeleted": true,
    "createdAt": "2026-04-03T04:32:32.721Z",
    "deletedAt": "2026-05-04T10:06:16.580Z"
  }
}
```

---

# 5. Восстановить файл

## `POST /api/v1/admin/orders/:orderId/files/:fileId/restore`

---

## Пример запроса

```http
POST /api/v1/admin/orders/0a617ff5-6346-45f8-8eb7-19f8a50b770b/files/243462a3-fd59-4c1b-88ed-b9228ef2a06c/restore
```

## Пример ответа

```json
{
  "success": true,
  "message": "OK",
  "data": {
    "id": "243462a3-fd59-4c1b-88ed-b9228ef2a06c",
    "orderId": "0a617ff5-6346-45f8-8eb7-19f8a50b770b",
    "messageId": null,
    "originalName": null,
    "type": "SOURCE",
    "name": "document.pdf",
    "storageKey": "files/doc.pdf",
    "size": null,
    "mime": null,
    "isDeleted": false,
    "createdAt": "2026-05-04T10:04:33.489Z",
    "deletedAt": null
  }
}
```
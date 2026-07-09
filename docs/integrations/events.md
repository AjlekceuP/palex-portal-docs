# Интеграционные сигналы

## Общая структура

Все события отправляются через очередь интеграции.

Пример:

```json
{
  "event": "onOrderHold",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "..."
    }
  }
}
```

### Сигнал: onOrderHold 

Отправляется при постановке заказа на паузу клиентом личного кабинета.  

В событие включаются только те языковые пары, для которых был изменен признак `ON_HOLD`.

### Поля

| Поле | Тип | Описание |
|------|-----|----------|
| ORDER_ID | String (UUID) | Идентификатор заказа |
| LANGUAGE_PAIRS | Array | Список измененных языковых пар |

### LANGUAGE_PAIRS

| Поле | Тип | Описание |
|------|-----|----------|
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары |
| ON_HOLD | Boolean | Новое значение признака паузы |


Пример:
```json
{
  "event": "onOrderHold",
  "payload": {
    "FIELDS": {
      "ORDER_ID": 5,
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": 3,
          "ON_HOLD": true
        },
        {
          "LANGUAGE_PAIR_ID": 5,
          "ON_HOLD": true
        }        
      ]
    }
  }
}
```
---


### Сигнал: onOrderResume

Отправляется при снятии паузы с заказа клиентом личного кабинета.

В событие включаются только те языковые пары, для которых был изменен признак `ON_HOLD`.

### Поля

| Поле | Тип | Описание |
|------|-----|----------|
| ORDER_ID | String (UUID) | Идентификатор заказа |
| LANGUAGE_PAIRS | Array | Список измененных языковых пар |

### LANGUAGE_PAIRS

| Поле | Тип | Описание |
|------|-----|----------|
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары |
| ON_HOLD | Boolean | Новое значение признака паузы |


Пример:
```json
{
  "event": "onOrderResume",
  "payload": {
    "FIELDS": {
      "ORDER_ID": 5,
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": 3,
          "ON_HOLD": false
        },
        {
          "LANGUAGE_PAIR_ID": 5,
          "ON_HOLD": false
        }
      ]
    }
  }
}
```
---

### Сигнал: onClientUpdate

Отправляется при изменении данных профиля клиента в личном кабинете:

- Имя (`firstName`)
- Фамилия (`lastName`)
- Компания (`company`)
- Отдел (`department`)
- Должность (`jobTitle`)
- Email (`email`)
- Телефон (`phone`)

### Поля


| Поле | Тип | Описание |
|------|-----|----------|
| USER_ID | String (UUID) | Идентификатор клиента |

Пример:
```json
{
  "event": "onClientUpdate",
  "payload": {
    "FIELDS": {
      "USER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71"
    }
  }
}
```
---


### Сигнал: onClientNotificationsUpdate

Отправляется при изменении настройки отправки уведомлений клиенту по электронной почте (`email_notifications`).

### Поля

| Поле | Тип           | Описание              |
|------|---------------|-----------------------|
| USER_ID | String (UUID) | Идентификатор клиента |
| EMAIL_NOTIFICATIONS | Boolean        | Новое значение настройки отправки уведомлений по электронной почте|

Пример:
```json
{
  "event": "onClientNotificationsUpdate",
  "payload": {
    "FIELDS": {
      "USER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "EMAIL_NOTIFICATIONS": true
    }
  }
}
```
---

### Сигнал: onClientRegistered

Отправляется после завершения регистрации клиентом по приглашению.

### Поля

| Поле | Тип | Описание |
|------|-----|----------|
| USER_ID | String (UUID) | Идентификатор клиента |

Пример:
```json
{
  "event": "onClientRegistered",
  "payload": {
    "FIELDS": {
      "USER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71"
    }
  }
}
```
---

### Сигнал: onOrderAdd

Отправляется после создания заказа клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание             |
|------|-----|----------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа |

Пример:
```json
{
  "event": "onOrderAdd",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71"
    }
  }
}
```
---


### Сигнал: onOrderUpdate

Отправляется после изменений данных заказа клиентом в личном кабинете:
- Номер отслеживания (`trackingNumber`)
- Название заказа (`productName`)
- Предпочтительная дата выполнения  (`preferAt`)
- Описание (`description`)
- Дополнительные примечания (`specialNotes`)

### Поля

| Поле | Тип | Описание             |
|------|-----|----------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа |

Пример:
```json
{
  "event": "onOrderUpdate",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71"
    }
  }
}
```
---

### Сигнал: onCommentAdd

Отправляется после создания сообщения к заказу клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание                |
|------|-----|-------------------------|
| MESSAGE_ID | String (UUID) | Идентификатор сообщения |
| ORDER_ID | String (UUID) | Идентификатор заказа    |

Пример:
```json
{
  "event": "onCommentAdd",
  "payload": {
    "FIELDS": {
      "MESSAGE_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "ORDER_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924"
    }
  }
}
```
---

### Сигнал: onOrderStatusUpdate

Отправляется после изменения статуса заказа клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание              |
|------|-----|-----------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа  |
| STATUS_ID | String (UUID) | Идентификатор статуса |

Пример:
```json
{
  "event": "onOrderStatusUpdate",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "STATUS_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924"
    }
  }
}
```
---

### Сигнал: onOrderClientManagerUpdate

Отправляется после изменения владельца заказа клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание |
|------|------|----------|
| ORDER_ID | String (UUID) | Идентификатор заказа |
| OLD_USER_ID | String (UUID) | Идентификатор предыдущего владельца заказа |
| NEW_USER_ID | String (UUID) | Идентификатор нового владельца заказа |

Пример:

```json
{
  "event": "onOrderClientManagerUpdate",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "OLD_USER_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924",
      "NEW_USER_ID": "04a940fd-63da-499a-8a33-5c4bb4abfe2a"
    }
  }
}
```
---

### Сигнал: onLanguagePairsAdd

Отправляется после добавления в созданном заказе языковых пар клиентом в личном кабинете.

### Поля

| Поле | Тип            | Описание                                             |
|------|----------------|------------------------------------------------------|
| ORDER_ID | String (UUID)  | Идентификатор заказа                                 |
| LANGUAGE_PAIRS | Array (Object) | Список добавленных языковых пар  |


### LANGUAGE_PAIRS

| Поле | Тип | Описание |
|------|------|----------|
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары |

Пример:

```json
{
  "event": "onLanguagePairsAdd",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924",
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": "8f3b0f67-5e18-4b89-8d90-9a74c9d93c5d"
        },
        {
          "LANGUAGE_PAIR_ID": "3ab6a11e-8a8f-4a3c-87ab-6d69b74d7b2d"
        }
      ]
    }
  }
}
```
---

### Сигнал: onLanguagePairStatusUpdate

Отправляется после изменения статуса языковой пары клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание                                   |
|------|------|--------------------------------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа                       |
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары                |
| STATUS_ID | String (UUID) | Идентификатор нового статуса языковой пары |

Пример:

```json
{
  "event": "onLanguagePairStatusUpdate",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "LANGUAGE_PAIR_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924",
      "STATUS_ID": "04a940fd-63da-499a-8a33-5c4bb4abfe2a"
    }
  }
}
```
---



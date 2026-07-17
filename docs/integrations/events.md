# Интеграционные сигналы

## Общая структура

Все события отправляются через очередь интеграции.

Пример:

```json
{
  "event": "...",
  "payload": {
    "FIELDS": { ... }
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
      "ORDER_ID": "2eb680ab-62ea-492d-b211-cd91de9533af",
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": "24dcdf3a-2a9f-4ecf-91cc-5b69d4e0916b",
          "ON_HOLD": true
        },
        {
          "LANGUAGE_PAIR_ID": "17ef304e-9ce2-4dcd-8c05-1d993aa56a46",
          "ON_HOLD": true
        }        
      ]
    }
  }
}
```
---


### Сигнал: onOrderResume

Отправляется:
-  при снятии паузы с заказа клиентом личного кабинета;
-  при снятии паузы с языковой пары, если при этом заказ также был снят с паузы.

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
      "ORDER_ID": "5c3f96f6-3a2d-4d4d-a0d0-b55d3fbe12a4",
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": "2322a06d-581d-418c-973e-c82063524659",
          "ON_HOLD": false
        },
        {
          "LANGUAGE_PAIR_ID": "7a66b63f-43e9-4f1d-a7cb-c88cb61cb7c9",
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
- Телефон (`phone`)
  
**Примечание:** изменение адреса электронной почты (`email`) в текущей версии не поддерживается и не приводит к отправке данного события.

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

В событие включаются только те языковые пары, для которых был изменен статус `STATUS_ID`.

### Поля

| Поле | Тип | Описание                       |
|------|-----|--------------------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа           |
| STATUS_ID | String (UUID) | Идентификатор статуса          |
| LANGUAGE_PAIRS | Array | Список измененных языковых пар |

### LANGUAGE_PAIRS

| Поле | Тип | Описание                            |
|------|-----|-------------------------------------|
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары         |
| STATUS_ID | String (UUID) | Идентификатор статуса языковой пары |


Пример:
```json
{
  "event": "onOrderStatusUpdate",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "STATUS_ID": "1fc6bdfc-755d-4f23-b198-4a7f65b9d924",
      "LANGUAGE_PAIRS": [
        {
          "LANGUAGE_PAIR_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
          "STATUS_ID": "c6f26ed6-9f47-4cca-b42e-51a6263b6ebc"
        },
        {
          "LANGUAGE_PAIR_ID": "cc2f75da-d831-426f-b2d7-28e3d53266cf",
          "STATUS_ID": "c6f26ed6-9f47-4cca-b42e-51a6263b6ebc"
        }
      ]      
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

| Поле | Тип           | Описание                                             |
|------|---------------|------------------------------------------------------|
| ORDER_ID | String (UUID) | Идентификатор заказа                                 |
| LANGUAGE_PAIRS | Array | Список добавленных языковых пар  |


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

Отправляется при изменении статуса языковой пары клиентом в личном кабинете.

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

### Сигнал: onLanguagePairHold

Отправляется после постановки языковой пары на паузу клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание |
|------|-----|----------|
| ORDER_ID | String (UUID) | Идентификатор заказа                       |
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары |
| ON_HOLD | Boolean | Новое значение признака паузы |

Пример:
```json
{
  "event": "onLanguagePairHold",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "LANGUAGE_PAIR_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "ON_HOLD": true
    }
  }
}
```
---

### Сигнал: onLanguagePairResume

Отправляется после снятия паузы с языковой пары клиентом в личном кабинете.

### Поля

| Поле | Тип | Описание |
|------|-----|----------|
| ORDER_ID | String (UUID) | Идентификатор заказа                       |
| LANGUAGE_PAIR_ID | String (UUID) | Идентификатор языковой пары |
| ON_HOLD | Boolean | Новое значение признака паузы |

Пример:
```json
{
  "event": "onLanguagePairResume",
  "payload": {
    "FIELDS": {
      "ORDER_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "LANGUAGE_PAIR_ID": "5d4d8d83-ec5a-4b4f-9c9f-9d54f7b33c71",
      "ON_HOLD": false
    }
  }
}
```
---
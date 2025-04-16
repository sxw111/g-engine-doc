# Документация API G-Engine

## Основной URL
`https://b2b-api.ggsel.com/api/v2.1`


---
## Аутентификация

### Получение access token

**Header:**
| Заголовок      | Значение      |
|---------------|----------|
| Content-Type         | application/x-www-form-urlencoded  |

**Тело запроса (form-data / x-www-form-urlencoded)**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| username	         | String  | Да          | 	Логин пользователя|
| password        | String  | Да          | 	Пароль пользователя                  |


**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/auth/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "username=your_login&password=your_password"
```

**Ответ 200 ОК**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}

```

## Валюты

### Получение курса валют
`GET /currencies`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры URL:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| source         | String  | Да          | Источник курса валют. Допустимые значения: `cb_rf`, `steam`|
| pair        | String  | Да          | Валютная пара. Допустимые значения: `USD:RUB`, `EUR:RUB`                 |


**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/currencies?source=cb_rf&pair=USD:RUB" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."

```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": {
    "source": "cb_rf",
    "pair": "USD:RUB",
    "currency_rate": 92.514
  }
}

```
## Финансы

### Получение данных о финансах
`GET /finances`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| funds_type    | String   | Нет          | Тип средств (balance, cashback)|
| limit         | Integer  | Нет          | Кол-во результатов (1-500, default 100)|
| offset        | Integer  | Нет          | Смещение (default 0)                  |
| sort_by       | String   | Нет          | Поле сортировки (напр. "date")        |
| sort_order    | String   | Нет          | Порядок сортировки (asc/desc)         |
| start_date    | Date     | Нет          | Начальная дата фильтра                |
| end_date      | Date     | Нет          | Конечная дата фильтра                 |
| search_field  | String   | Нет          | Поле для поиска                       |
| search_value  | String   | Нет          | Значение для поиска                   |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/finances?funds_type=balance&limit=10&sort_order=desc" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": [
    {
      "id": 101,
      "code": "TXN-456789",
      "amount": 500.0,
      "fee": 5.0,
      "delta": 495.0,
      "currency": "RUB",
      "created_at": "2025-04-10T15:30:00",
      "user_id": 32,
      "funds_type": "balance"
    },
    {
      "id": 102,
      "code": "TXN-456790",
      "amount": -150.0,
      "fee": 0.0,
      "delta": -150.0,
      "currency": "RUB",
      "created_at": "2025-04-09T12:15:00",
      "user_id": 32,
      "funds_type": "balance"
    }
  ]
}
```
## Транзакции

### Получение списка транзакций
`GET /transactions`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| limit         | Integer  | Нет          | Кол-во результатов (1-500, default 100)|
| offset        | Integer  | Нет          | Смещение (default 0)                  |
| sort_by       | String   | Нет          | Поле сортировки (напр. "date")        |
| sort_order    | String   | Нет          | Порядок сортировки (asc/desc)         |
| start_date    | Date     | Нет          | Начальная дата фильтра                |
| end_date      | Date     | Нет          | Конечная дата фильтра                 |
| search_field  | String   | Нет          | Поле для поиска                       |
| search_value  | String   | Нет          | Значение для поиска                   |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/transactions?limit=10&sort_order=desc" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": [
    {
      "date": "2025-03-20T10:31:21",
      "issue_date": "2025-03-20T10:41:48",
      "amount": 30.0,
      "amount_usd": 0.3292,
      "status": "Баланс успешно пополнен",
      "status_code": "PAYMENT_SUCCESS",
      "children": []
    }
  ]
}
```

## Пользователи

### Получение списка пользователей
`GET /users`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| limit         | Integer  | Нет          | Кол-во результатов (1-500, default 100)|
| offset        | Integer  | Нет          | Смещение (default 0)                  |
| sort_by       | String   | Нет          | Поле сортировки (напр. "date")        |
| sort_order    | String   | Нет          | Порядок сортировки (asc/desc)         |
| start_date    | Date     | Нет          | Начальная дата фильтра                |
| end_date      | Date     | Нет          | Конечная дата фильтра                 |
| search_field  | String   | Нет          | Поле для поиска                       |
| search_value  | String   | Нет          | Значение для поиска                   |
| role_name     | String   | Нет          | Фильтр по роли                        |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/users?limit=10&sort_order=desc" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": [
    {
      "id": 12,
      "login": "john_doe",
      "name": "John",
      "last_name": "Doe",
      "role": {
        "name": "sales"
      },
      "created_at": "2025-03-20T14:12:55",
      "is_active": true,
      "children": [],
      "stores": [],
      "parameters": [],
      "wallets": [],
      "user_settings": null,
      "integration": null
    }
  ]
}
```

### Получение баланса пользователя
`GET /users/balance`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| limit         | Integer  | Нет          | Кол-во результатов (1-500, default 100)|
| offset        | Integer  | Нет          | Смещение (default 0)                  |
| sort_by       | String   | Нет          | Поле сортировки (напр. "date")        |
| sort_order    | String   | Нет          | Порядок сортировки (asc/desc)         |
| start_date    | Date     | Нет          | Начальная дата фильтра                |
| end_date      | Date     | Нет          | Конечная дата фильтра                 |
| search_field  | String   | Нет          | Поле для поиска                       |
| search_value  | String   | Нет          | Значение для поиска                   |
| role_name     | String   | Нет          | Фильтр по роли                        |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/users/balance" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": [
    {
      "currency": "RUB",
      "balance": 12500.0,
      "cashback": 120.5
    },
    {
      "currency": "USD",
      "balance": 100.0,
      "cashback": 0.0
    }
  ]
}
```

### Получение данных текущего пользователя
`GET /users/me`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/users/me" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."

```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": {
    "name": "John",
    "last_name": "Doe",
    "role": {
      "id": 1,
      "name": "admin"
    },
    "is_active": true,
    "wallets": [
      {
        "id": 1,
        "balance": 1000.0,
        "cashback": 50.0,
        "currency": "RUB"
      },
      {
        "id": 2,
        "balance": 150.0,
        "cashback": 10.0,
        "currency": "USD"
      }
    ],
    "user_settings": {
      "id": 1,
      "route_version": "v1.0",
      "is_beta": false
    },
    "integration": {
      "id": 1,
      "pub_token": "abcdef123456",
      "digiseller": {
        "id": 1,
        "seller_id": 123,
        "api_guid": "guid_value",
        "seller_pass": "secret_pass"
      }
    }
  }
}
```

## Платежи

### Создание и верификация платежа
`POST /payments/verify`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Тело запроса:**
```json
{
  "transaction_id": "abc123xyz456",
  "service_id": 123,
  "account": "example_account",
  "amount": 100.50,
  "currency": "USD"
}
```


**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/payments/verify" \
-H "Content-Type: application/json" \
-d '{
  "transaction_id": "abc123xyz456",
  "service_id": 123,
  "account": "example_account",
  "amount": 100.50,
  "currency": "USD"
}'
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": {
    "transaction_id": "abc1234567890",
    "amount_rub": 100.50,
    "amount_usd": 1.34,
    "account": "example_account",
    "date": "2025-04-11T10:31:21",
    "issue_date": "2025-04-11T10:41:48",
    "status": "PAYMENT_SUCCESS",
    "status_code": "PAYMENT_SUCCESS"
  }
} 
```


### Выполнение платежа
`POST /payments/{transaction_id}/execute`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры URL:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| transaction_id         | String  | Да        | Уникальный идентификатор транзакции|


**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/payments/abc123xyz456/execute" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "Платёж успешно выполнен",
  "data": {
    "transaction_id": "abc123xyz456",
    "amount_rub": 100.50,
    "amount_usd": 1.34,
    "account": "example_account",
    "date": "2025-04-11T10:31:21",
    "issue_date": "2025-04-11T10:41:48",
    "status": "PAYMENT_SUCCESS",
    "status_code": "PAYMENT_SUCCESS"
  }
}
```

### Получение информации о статусе платежа
`GET /payments/{transaction_id}/status`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры URL:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| transaction_id         | String  | Да        | Уникальный идентификатор транзакции|


**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/payments/abc123xyz456/status" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "OK",
  "data": {
    "status": "PAYMENT_SUCCESS",
    "status_code": "PAYMENT_SUCCESS"
  }
}
```

## Заказы

### Получение списка заказов
`GET /shop/orders`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| limit         | Integer  | Нет          | Кол-во результатов (1-500, default 100)|
| offset        | Integer  | Нет          | Смещение (default 0)                  |
| sort_by       | String   | Нет          | Поле сортировки (напр. "date")        |
| sort_order    | String   | Нет          | Порядок сортировки (asc/desc)         |
| start_date    | Date     | Нет          | Начальная дата фильтра                |
| end_date      | Date     | Нет          | Конечная дата фильтра                 |
| search_field  | String   | Нет          | Поле для поиска                       |
| search_value  | String   | Нет          | Значение для поиска                   |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/orders?limit=50&sort_by=date" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": [
    {
      "id": 1,
      "wallet_id": 2,
      "price": 100.5,
      "status": "pending",
      "created_at": "2024-01-01T12:00:00",
      "updated_at": "2024-01-02T14:00:00"
    }
  ]
}
```

### Получение информации о заказе
`GET /shop/orders/{order_id}`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры URL:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| order_id         | Integer  | Да          | ID заказа|

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/shop/orders/123" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": {
    "id": 123,
    "wallet_id": 3,
    "price": 200.0,
    "status": "paid",
    "created_at": "2024-01-01T10:00:00",
    "updated_at": "2024-01-01T11:00:00",
    "order_items": [
      {
        "id": 10,
        "order_id": 123,
        "denomination_id": 9,
        "activation_code": "KEY-123456-7890",
        "price": 200.0
      }
    ]
  }
}
```

### Создание заказа
`POST /shop/orders`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Тело запроса**
```json
{
  "wallet_id": 1,
  "purchases": [
    {
      "denomination_id": 5,
      "quantity": 2
    }
  ]
}
```

**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/shop/orders" \
-H "Authorization: Bearer <access_token>" \
-H "Content-Type: application/json" \
-d '{
  "wallet_id": 1,
  "purchases": [
    { "denomination_id": 5, "quantity": 2 }
  ]
}'
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": {
    "id": 456,
    "wallet_id": 1,
    "price": 300.0,
    "status": "pending",
    "created_at": "2024-01-03T10:00:00",
    "updated_at": null,
    "order_items": [
      {
        "id": 22,
        "order_id": 456,
        "denomination_id": 5,
        "price": 150.0
      }
    ]
  }
}
```


### Оплата заказа
`POST /shop/orders/{order_id}/pay`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Параметры URL:**
| Параметр      | Тип      | Обязательный | Описание                              |
|---------------|----------|--------------|---------------------------------------|
| order_id         | Integer  | Да          | ID заказа|

**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/shop/orders/456/pay" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": {
    "id": 456,
    "wallet_id": 1,
    "price": 150.0,
    "status": "shipped",
    "created_at": "2024-01-03T10:00:00",
    "updated_at": "2024-01-03T10:05:00",
    "order_items": [
      {
        "id": 22,
        "order_id": 456,
        "denomination_id": 5,
        "activation_code": "DELIV-KEY-XYZ",
        "price": 150.0
      }
    ]
  }
}
```

## Продукты

### Получение списка продуктов
`GET /shop/products`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Пример запроса:**
```bash
curl -X GET "https://b2b-api.ggsel.com/api/v2.1/shop/products" \
-H "Authorization: Bearer <access_token>"
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": [
    {
      "id": 1,
      "name": "Product A",
      "description": "Описание продукта",
      "instruction": "Инструкция к применению",
      "service_number": 100,
      "category": "software",
      "denominations": [
        {
          "id": 5,
          "name": "500 рублей",
          "product_id": 1,
          "value": "500",
          "price": 480.0,
          "rp": 5.0
        }
      ]
    }
  ]
}
```

## Сток

### Проверка остатков
`POST /shop/stock`

**Заголовки:**
| Заголовок       | Значение             |
|-----------------|----------------------|
| Authorization  | Bearer \<token\>     |

**Тело запроса**
```json
{
  "denomination_ids": [int]
}
```

**Пример запроса:**
```bash
curl -X POST "https://b2b-api.ggsel.com/api/v2.1/shop/stock" \
-H "Authorization: Bearer <access_token>" \
-H "Content-Type: application/json" \
-d '{"denomination_id": 5}'
```

**Ответ 200 ОК**
```json
{
  "success": true,
  "message": "",
  "data": {
    "stock": [
      {
        "denomination_id": 5,
        "stock_count": 23
      }
    ]
  }
}
```

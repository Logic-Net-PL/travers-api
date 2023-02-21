# API Structure
## API / Token Check
Request

`GET /healthcheck`

Response

```json
{
  "ping": "pong",
  "timestamp": "1675717124"
}
```

## Available markets list
List of markets available for API usage

Request

`GET /markets`

Response

```json
{
  "success": true,
  "data": [
    {
      "name": "MEDIA MARKT KATOWICE I 3 STAWY",
      "street": "ALPEJSKA 6",
      "city": "Katowice",
      "postcode": "40-506",
      "sap_code": "O2243"
    }
  ]
}
```

## Available delivery types
List of delivery types available for API usage

Request
`GET /delivery_types`

Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Dostawa Standard"
    }
  ]
}
```

## Available delivery ranges
List of delivery ranges available for API usage

Request
`GET /delivery_ranges`

Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "range": "10-15"
    }
  ]
}
```

## Available Service Codes
List of service codes available for API usage for particular one customer. Different customers
can use different (customized) codes. MDI core codes are same for all customers.

Request

`GET /service_codes`

Payload

```json
{
  "customer_code": "O2243"
}
```

Response

```json
{
  "success": true,
  "data": [
    {
      "code": "1447629",
      "description": "INSTALACJA AGD KUCHNIE PODSTAWOWY",
      "mdi_group": "KTCH_BSC"
    }
  ]
}
```

## Create Task
Delivery task creation

Request

`PUT /task`

Payload

```json
{
  "market_code": "O2243",
  "delivery_number": "DN001",
  "customer_name": "Joe Doe",
  "customer_phone": "+48 111 222 333",
  "delivery_address": "Delivery Street 1/2",
  "delivery_postcode": "02-001",
  "delivery_city": "Customer City",
  "additional_info": "Please contact customer before delivery.",
  "is_recyclable": 0,
  "delivery_type": 3,
  "delivery_range": 1,
  "delivery_date": "2023-02-13",
  "is_paid": 1,
  "amount_to_pay": 0,
  "product_name": [
    "Product Name 1",
    "Product Name 2"
  ],
  "quantity": [
    1,
    2
  ],
  "product_receive_name": [
    "Product Receive Name 1",
    "Product Receive Name 2"
  ],
  "quantity_receive": [
    1,
    2
  ],
  "service_name": [
    "KTCH_BSC"
  ]
}
```

Response

```json
{
  "success": true
}
```

## Cancel task
Task can be cancelled until delivery will be started by courier.

Request

`DELETE /task`

Payload

```json
{
  "delivery_number": "DN001"
}
```

Response

```json
{
  "success": true,
  "errors": []
}
```

## Task Basic List

Simple method to get basic details about delivery task/tasks created by API based on payload parameters

Request

`GET /tasks_list`

Payload (__At least one parameter is required__)

```json
{
  "delivery_number": "DN001",
  "delivery_date": "2023-02-13"
}
```

Response

```json
{
  "success": true,
  "data": [
    {
      "delivery_number": "DN001",
      "status": "Zlecenie anulowane"
    }
  ]
}
```

## Task Detailed List
Simple method to get detailed information about delivery task/tasks created by API based on payload parameters

Request

`GET /tasks_list_detailed`

Payload (__At least one parameter is required__)

```json
{
  "delivery_number": "DN001",
  "delivery_date": "2023-02-13"
}
```

Response

```json
{
  "success": true,
  "data": [
    {
      "market_code": "O2243",
      "delivery_number": "DN001",
      "customer_name": "Joe Doe",
      "delivery_address": "Delivery Street 1/2",
      "delivery_postcode": "02-001",
      "delivery_city": "Customer City",
      "additional_info": "Please contact customer before delivery.",
      "is_recyclable": 0,
      "delivery_type": {
        "id": 3,
        "name": "Super Express"
      },
      "status": "Zlecenie anulowane",
      "delivery_range": 1,
      "delivery_date": "2023-02-04",
      "is_paid": 1,
      "amount_to_pay": 0,
      "products_sent": [
        {
          "product_name": "Product Name 1",
          "quantity": 1
        },
        {
          "product_name": "Product Name 2",
          "quantity": 2
        }
      ],
      "products_received": [
        {
          "product_name": "Product Receive Name 1",
          "quantity": 1
        },
        {
          "product_name": "Product Receive Name 2",
          "quantity": 2
        }
      ],
      "services": [
        {
          "mdi_group": "KTCH_BSC",
          "description": "INSTALACJA AGD KUCHNIE PODSTAWOWY",
          "is_done": "Niewykonana"
        }
      ]
    }
  ]
}
```

## API Objects
Definition of key API objects

### Market
| Property     | Type   | Description                                       |
|--------------|--------|---------------------------------------------------|
| `$.name`     | string | Market name                                       |
| `$.street`   | string | Market address 1                                  |
| `$.city`     | string | Market address 2                                  |
| `$.postcode` | string | Market address 3                                  |
| `$.sap_code`  | string | SAP Code (MDI Code) for future integration usage. |

### Delivery Type
| Property | Type    | Description                                       |
|---------|---------|---------------------------------------------------|
| `$.id`  | int(11) | Delivery type id for future integration usage     |
| `$.name` | string  | Delivery type name                                |

### Delivery Range
| Property  | Type    | Description                                    |
|-----------|---------|------------------------------------------------|
| `$.id`    | int(11) | Delivery range id for future integration usage |
| `$.range` | string  | Delivery range                                 |

### Service Code
| Property        | Type    | Description                                       |
|-----------------|---------|---------------------------------------------------|
| `$.code`        | string  | Service code used internally                      |
| `$.description` | string  | Service description                               |
| `$.mdi_group`   | string  | Service MDI Code                                  |

### Task
| Property                     | Type           | Required | Description                                                     |
|------------------------------|----------------|----------|-----------------------------------------------------------------|
| `$.market_code`              | string         | YES      | Please check _Market_                                           |
| `$.delivery_number`          | string         | YES      | MDI Delivery Number                                             |
| `$.customer_name`            | string         | YES      | Customer Name                                                   |
| `$.customer_phone`           | string         | YES      | Customer Contact Number                                         |
| `$.delivery_address`         | string         | YES      | Delivery address 1                                              |
| `$.delivery_postcode`        | string         | YES      | Delivery address 2                                              |
| `$.delivery_city`            | string         | YES      | Delivery address 3                                              |
| `$.additional_info`          | string         | YES      | Additional information for delivery                             |
| `$.is_recyclable`            | tinyint(1)     | YES      | Mark in case if courier should receive some goods from customer |
| `$.delivery_type`            | int(11)        | YES      | Please check _Delivery type_                                    |
| `$.delivery_range`           | int(11)        | YES      | Please check _Delivery range_                                   |
| `$.delivery_date`            | datetime:Y-m-d | YES      | Delivery date (format YYYY-MM-DD)                               |
| `$.is_paid`                  | tinyint(1)     | YES      | Mark in case if goods already paid by customer                  |
| `$.amount_to_pay`            | float          | YES      | Feel in case if goods are not paid by customer                  |
| `$.product_name.[*]`         | array:string   | NO       | Array of products names                                         |
| `$.quantity.[*]`             | array:int(11)  | NO       | Array of products quantities                                    |
| `$.product_receive_name.[*]` | array:string   | NO       | Array of products names to be received from customer            |
| `$.quantity_receive.[*]`     | array:int(11)  | NO       | Array of products quantities to be received from customer       |
| `$.service_name.[*]`         | array:string   | NO       | Array of service codes. Please check _Service code_             |

### Task Basic
| Property            | Type   | Description             |
|---------------------|--------|-------------------------|
| `$.delivery_number` | string | Delivery number         |
| `$.status`          | string | Current delivery status |

### Task Detailed
| Property                  | Type           | Description                                                     |
|---------------------------|----------------|-----------------------------------------------------------------|
| `$.market_code`           | string         | Please check _Market_                                           |
| `$.delivery_number`       | string         | MDI Delivery Number                                             |
| `$.customer_name`         | string         | Customer Name                                                   |
| `$.customer_phone`        | string         | Customer Contact Number                                         |
| `$.delivery_address`      | string         | Delivery address 1                                              |
| `$.delivery_postcode`     | string         | Delivery address 2                                              |
| `$.delivery_city`         | string         | Delivery address 3                                              |
| `$.additional_info`       | string         | Additional information for delivery                             |
| `$.is_recyclable`         | tinyint(1)     | Mark in case if courier should receive some goods from customer |
| `$.delivery_type`         | object         | Please check _Delivery type_                                    |
| `$.status`                | string         | Current delivery status                                         |
| `$.delivery_range`        | int(11)        | Please check _Delivery range_                                   |
| `$.delivery_date`         | datetime:Y-m-d | Delivery date (format YYYY-MM-DD)                               |
| `$.is_paid`               | tinyint(1)     | Mark in case if goods already paid by customer                  |
| `$.amount_to_pay`         | float          | Feel in case if goods are not paid by customer                  |
| `$.products_sent.[*]`     | array:object   | Please check _Product sent_                                     |
| `$.products_received.[*]` | array:object   | Please check _Product received_                                 |
| `$.services.[*]`          | array:object   | Please check _Service_                                          |

### Product sent
| Property         | Type    | Description      |
|------------------|---------|------------------|
| `$.product_name` | string  | Product name     |
| `$.quantity`     | int(11) | Product quantity |

### Product received
| Property          | Type    | Description      |
|-------------------|---------|------------------|
| `$.product_name`  | string  | Product name     |
| `$.quantity`      | int(11) | Product quantity |

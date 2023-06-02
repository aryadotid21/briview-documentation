# 📚 BRIview 2023 Documentation

Welcome to the BRIview 2023 Documentation! 🎉 Here, you'll find detailed information about the REST API Data Delivery. This document aims to explain the various aspects of data delivery through our API. From endpoints to data formats, we've got you covered. Let's dive in and unlock the power of BRIview together! 💪

## 📖 API Reference

### Send Automatic Ticket (Agent Base) ✉️

```http
  POST http://{{host}}:3232/api/tiket
```

#### 📝 Preperation 
| Header   | Description                                      |
| :------- | :----------------------------------------------- |
| `Header` | **Required**. Key: Authorization, Value: JWT Key |
| `body`   | Raw JSON                                         |

#### 📋 Parameter

| Parameter       | Type     | Description                                                                                                       |
| :-------------- | :------- | :---------------------------------------------------------------------------------------------------------------- |
| `no_tiket`      | `string` | **Required**. The ticket number from the vendor                                                                   |
| `tid`           | `string` | **Required**. Terminal ID or machine ID                                                                           |
| `error`         | `string` | **Required**. The error or problem displayed by the machine. The error should be appropriate to its service level |
| `status`        | `string` | **Required**. The current status of the ticket. Example: "OPEN,REQ_CLOSE,DISPATCH,etc."                           |
| `service_level` | `string` | **Required**. Service level. Example: "FLM/SLM"                                                                   |
| `created_at`    | `string` | **Required**. Datetime in the format "YYYY-MM-DD HH:MM:SS"                                                        |


#### ▶️ Usage/Examples

```
curl --location --request POST 'http://{{host}}:3232/api/tiket' \
--header 'Authorization: {token}' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "no_tiket": "A2022122216000212",
        "tid": "160002",
        "error": "PRINTER ERROR",
        "status": "OPEN",
        "service_level": "FLM",
        "created_at": "2022-12-22 08:58:14"
    }
]'
```

#### 🔍 Example Response
```
{
  "draw": 0,
  "recordsTotal": 1,
  "recordsFiltered": 0,
  "status": 200,
  "error": "",
  "message": "OK, Success Create Tiket",
  "data": [
    {
      "Tiket berhasil di insert": [
        "A2022122216000212"
      ],
      "Tiket gagal di insert": null
    }
  ]
}
```


### Send Machine Status (Agent Base) 📊

```http
  POST http://{{host}}:3232/api/status-mesin
```

#### 📝 Preperation
| Header   | Description                                      |
| :------- | :----------------------------------------------- |
| `Header` | **Required**. Key: Authorization, Value: JWT Key |
| `body`   | Raw JSON                                         |

#### 📋 Parameter

| Parameter          | Type     | Description                                                                            |
| :----------------- | :------- | :------------------------------------------------------------------------------------- |
| `tid`              | `string` | **Required**. Terminal ID or machine ID                                                |
| `atm_status`       | `string` | **Required**. The status of the machine. Example: "IN_SERVICE"                         |
| `last_amount`      | `int`    | **Required**. The last amount of the machine                                           |
| `casette_1_A`      | `int`    | **Required**. The amount in cassette 1 for type A                                      |
| `casette_2_A`      | `int`    | **Required**. The amount in cassette 2 for type A                                      |
| `casette_3_B`      | `int`    | **Required**. The amount in cassette 3 for type B                                      |
| `casette_4_B`      | `int`    | **Required**. The amount in cassette 4 for type B                                      |
| `last_transaction` | `string` | **Required**. The datetime of the last transaction in the format "YYYY-MM-DD HH:MM:SS" |


#### ▶️ Usage/Examples

```
curl --location --request POST 'http://xxx:3232/api/status-mesin' \
--header 'Authorization: {token}' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
      "tid": "002016",
      "atm_status": "IN_SERVICE",
      "last_amount": 1,
      "casette_1_A": 1368,
      "casette_2_A": 1000,
      "casette_3_B": 605,
      "casette_4_B": 100121,
      "last_transaction": "2023-01-06 17:33:15"
    }
  ]'

```

#### 🔍 Example Response
```
{
  "draw": 0,
  "recordsTotal": 0,
  "recordsFiltered": 0,
  "status": 200,
  "error": "",
  "message": "OK, Success Create or Update Machine Status",
  "data": null
}
```
## 📖 Response Description

| Status   |  Description  |
| ------------- |:--------------|
|`200`| ✅ OK: Everything is working fine. The requested resource has been successfully fetched and transmitted in the message body.|
|`400`| ❌ BAD REQUEST: The provided request was invalid or cannot be served. Refer to the error payload for more details on the exact error encountered.|
|`401`| 🔒 UNAUTHORIZED: User authentication is required to access the requested resource. Please ensure proper authentication credentials are provided.|
|`404`| 🔍 NOT FOUND: The specified URI does not correspond to any existing resource. Double-check the URI for accuracy.|
|`500`| ❗ INTERNAL SERVER ERROR: An unexpected error occurred in the API's global catch block. The stack trace should be logged internally and not returned as a response. Our team is actively working to resolve the issue. |

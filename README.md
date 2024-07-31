# ATH Móvil Technical Documentation
for ATH Móvil Payment Button API.

# Change Log

| Date | Changes | Comments |
| --- | --- | --- |
| 04/12/2023 | Initial version 1.0 |
| 07/17/2024 | Version 1.2 |  General information related to ATH Business & ATH Móvil with instructions on how to open an account. |

# Table of Contents

**[Change Log 2](#_Toc133417448)**

**[Overview 4](#_Toc133417449)**

**[Prerequisites 4](#_Toc133417450)**

**[Customer Experience 5](#_Toc133417451)**

**[Payment Service 6](#_Toc133417452)**


**[Find Payment Service 11](#_Toc133417455)**

**[Authorization Service 10](#_Toc133417454)**

**[Transaction Expired or Canceled Response: Status CANCEL 15](#_Toc133417456)**

**[Update Payment (Phone Number) 15](#_Toc133417457)**

**[Refund Payment 16](#_Toc133417458)**

**[Cancel Payment 17](#_Toc133417459)**

**[Error Messages 18](#_Toc133417460)**

**[Reporting 22](#_Toc133417461)**

**[Other information 23](#_Toc133417462)**

## Overview

![Shape1](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

The ATH Móvil Payment Button is an API REST based application implemented to support ecommerce payments for ATH Business merchants. The API is available for integration, but the merchant must have an active ATH Business account with an active ATH card to receive payments.

Ours clients that use the Payment Button (PB) will be able to integrate each of these granular services into their business components and/or applications. This code works with HTTP protocols offered by REST based interfaces that are implemented through a separately web-based API layer. They can take advantage of multiple deployable service components, scalability, and a high degree of application and component decoupling provided by the API.

The API called for this JavaScript code is build based on JWT protocol to securely authenticate the communication between our services.

Disclaimer: The Payment Button ATH Móvil is not compatible with any major Ecommerce platform. This includes Shopify, Wix, Woocommerce or Stripe.

Disclaimer: We currently **do not** have a **Testing environment**. You need to have an active ATH Business account and a active ATH Móvil account.


## Prerequisites

![Shape2](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

Before using the ATH Móvil's payment you need to have:

### ATH Business

1\. An active ATH Business account.

2\. A card registered in your ATH Business profile. 

3\. The public and private key assigned to your business.

For instructions on how to open a ATH Business account please refer to: [ATHB flyer eng letter 1.pdf](https://github.com/user-attachments/files/16267504/ATHB.flyer.eng.letter.1.pdf)

For more information related to ATH Business and how it works please refer to:[ATH BUSINESS_Apr2024.pptx](https://github.com/user-attachments/files/16267585/ATH.BUSINESS_Apr2024.pptx)

### ATH Móvil

To complete the payment for testing purposes you need to have:

1\. An active ATH Móvil account.

2\. A card registered in your ATH Móvil profile. It can not be the same card that is registered in ATH Business.

For more information related to ATH Móvil and how it works please refer to:[ATH Móvil_Apr2024.pptx](https://github.com/user-attachments/files/16267592/ATH.Movil_Apr2024.pptx)

To start working with the API´s for ATH Móvils Payment Button with all its services, it is mandatory to have a Public Token per each business. This Public Token is found in the settings section of the ATH Business app and is assigned one unique token per ATH Business account.**

**ATH Business Settings:**

![Setting1](https://github.com/evertec/ATHM-Payment-Button-API/assets/99409598/f48d3f66-0659-41ab-b2ca-dc26e108575c) ![Setting2](https://github.com/evertec/ATHM-Payment-Button-API/assets/99409598/66f310c5-caa5-4d06-91b9-ac5a93d367e9)




**HTTPS**

Is important to highlight that all the exposed services from API must be called by using HTTPS protocol.

If you try to call any exposed service from API without using HTTPS protocol, you'll receive an error stating that it is mandatory the usage HTTPS protocol in your requests.

All the requests sent to the API should be in a simple and concise format like: Json (JavaScript Object Notation) that is a lightweight data-interchange format.

## Customer Experience

![Shape3](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

The new version of the payment button will introduce more security and synchronization with both the merchant and the customer. Here you can see a brief diagram on the high-level flow of the transaction:


<img width="468" alt="Picture1" src="https://github.com/evertec/ATHM-Payment-Button-API/assets/99409598/95e8d2c9-c43b-4061-bb38-5f342821524c">

##



## Payment Service

![Shape4](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

The Payment Service is the first step in the payment button's process. This service creates the ticket order or amount to be paid. This service "/payment" requires a payload with two mandatory attributes, "publicToken" and "total", which will be validated by the same service.

**Endpoint:** https://payments.athmovil.com/api/business-transaction/ecommerce/payment

**Headers:** Content-type – application/json

curl --location --request POST 'HOST/api/business-transaction/ecommerce/payment' \

--header 'Accept: application/json' \

--header 'Content-Type: application/json' \

--data-raw '{

"env": "production",

**Request**

- **publicToken** : This public token is the identifier of each business registered for the use of the payment button.
- **total** : The total is the amount that the user who just invoked the payment button must pay the merchant.
- **Timeout** : Time limit before the service responds with timeout error.

```

{
"publicToken": "a66ce73d04f2087615f6320b724defc5b4eedc55",

"timeout": "5000",

"total": "10",

"tax": "1",

"subtotal": "9",

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": "1",

"price": "10",

"tax": "1",

"metadata": "Diego"

}

],

"phoneNumber": "5613305493"

}'

```

**Detail:**

| **Variable** | **Data type** | **Required** | **Values** | **Description** |
| --- | --- | --- | --- | --- |
| PublicToken | String | YES | Business account public token | Determines the business account that the payment will be sent to. |
| Timeout | Number | No | Number between 120 and 600. | Time limit before the service responds with timeout error. Default value is set to 600 seconds (10 mins). |
| Total | Number | Yes | From 1.00 to 1500.00 | Total amount to be paid by the end user. |
| Tax | Number | No || Optional variable to display the payment tax (if applicable). |
| Subtotal | Number | No || Optional variable to display the payment tax (if applicable). |
| Metadata1 | String | Yes || variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.|
| Metadata2 | String | Yes || variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters. |
| Items | Array | Yes || Optional variable to display the items that the user is purchasing on ATH Móvil's payment screen. _metadata and tax are required but they can be set as null._ |
| phoneNumber | Number | Yes || The phone number registered to the ATH Móvil account where the payment is going to be sent to. |

**Response**

- **ecommerceId** : This ID represent the ticket of the transaction to be paid with the information provided in the request.
- **auth\_token** : This token identifies the transaction authorized by the user pending to be submitted by the ecommerce app.

```

{

    "status": "success",

    "data": 
    {

        "ecommerceId": "0ab257f5-3837-11ed-855f-9b4af5a393ce",

        "auth\_token": "eyJraWQiOiJVVTZpdEt0U0N5Tm9abUFkWkt3ZGxpSDE2SDk5SXI5clNFeXJwbzh6dVVFIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIyZDE3ZmUzZjg4ZWNkYTZiZTcxOTdjOWFiNGVhNmU3OTI4ZmU0NDk1ZWVjOWU1M2MzZTg1ZDIyMWE5MjU2NjM2NDcyMjNmZDA0ODBhNjg4YjU0MWNkMGJmODI3OWI2YjAiLCJmaUlkIjoiIiwibmJmIjoxNjYzNjA0NDU4LCJhenAiOltdLCJwZXJtaXNzaW9ucyI6WyJjdXN0b21lci5idXNpbmVzcy5lY29tbWVyY2UuYXV0aG9yaXphdGlvbjp3cml0ZSJdLCJpc3MiOiJQcm9jZXNzIFBheW1lbnQiLCJzY29wZXMiOlsiY3VzdG9tZXIuYnVzaW5lc3MuZWNvbW1lcmNlLmF1dGhvcml6YXRpb246d3JpdGUiXSwiZXhwIjoxNjYzNjA0NDU4LCJpYXQiOjE2NjM2MDQ0NTh9.ffZ5NWgyb-87f\_qVnytfsJl-nfDGB3mB98HwceORLgC6cs\_S4PgMYlbhMsrIacOG7Vz3LatT55o1YShiGRgsk8lwNDl\_v-tSwIspGLO2kHdYd4oBrD4Y5nWpFgsZ6zYkjVPyM5ZqUlH\_nVSlDGKm6HevqunSl\_D6pgAA25bd5MJ\_3M5FArMbM-iVDLpj6k4X3WRJZvSenYBAYczl6c\_BziuMprx6g0R9nDdzmtj2-4QGQJ5Tf2CP7Yj5A15M-z\_lDROcvMISzuR-2OVXpp5NbLEuu\_4TO4P6SoFOASksb3CFQC62YVIUH8E9hf0Sgocti0CHR4E9D25JFQMytXvfcA"

    }

```
## Find Payment Service

![Shape7](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

This service can be used to find the status of a transaction. This service is required to verify if the transaction has been **"CONFIRMED"** by the **ATH Móvil user** and the request the **/authorization** service. This service "/business/findPayment" requires a payload with two mandatory attributes "ecommerceId" and "publicToken", which will be validated by the same service.

**Endpoint:** https://payments.athmovil.com/api/business-transaction/ecommerce/business/findPayment

**Headers:** Content-type – application/json

**Request**

- **ecommerceId** : This ID represent the ticket of the transaction to be paid with the information provided in the request.
- **publicToken** : Determines the business account that the payment will be sent to.

curl --location --request POST 'https://vpce-04edaf73e4e83adea-flbxnqbx.execute-api.us-east-1.vpce.amazonaws.com/api/business-transaction/ecommerce/business/findPayment' \

--header 'Host: ozm9fx7yw5.execute-api.us-east-1.amazonaws.com' \

--header 'Accept: application/json' \

--header 'Authorization: Bearer \

--header 'Content-Type: application/json' \

--data-raw '{

"ecommerceId": "177a50fd-39fb-11ed-8b3d-230262020527",

"publicToken": "a66ce73d04f2087615f6320b724defc5b4eedc55",

}'

**Response**

- **status** : Confirm status of the service response.
- **ecommerceStatus** : represents the status of the ecommerce transaction.
- **transactionDate** : Authorization date
- **referenceNumber** : Unique transaction identifier
- **dailyTransactionID** : ID count for the transaction in the day.
- **businessName** : Buiness Name for the ATH Business account
- **businessPath** : Business Path for the ATH Business account
- **industry** : Industry of the business
- **total** : Total amount of the transaction
- **tax** : Tax to be charged in the transaction.
- **subtotal** : Subtotal amount of the transaction.
- **fee** : Fee to be charged in the transaction.
- **netAmount** : Net amount of the transaction
- **totalRefundedAmount** : amount to be refunded from the original transaction.
- **metadata1** : variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- **metadata2** : variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- **items** : Items paid in the transaction.

## Completed transaction (/Payment +/Confirmed & /Authorize) Response: Status **COMPLETED**
```

{

"status": "success",

"data": {

"ecommerceStatus": "COMPLETED",

"ecommerceId": "730e2c49-9387-11ed-8f43-c31784ccfc6c",

"referenceNumber": "215070443-402894c185ab1be40185acfe61c2000b",

"businessCustomerId": "402894d56e713892016e7f2963de0010",

"transactionDate": "2023-01-13 16:17:06",

"dailyTransactionId": "0006",

"businessName": "Tdameritrade",

"businessPath": "Tdameritrade",

"industry": "ENTERTAINMENT",

"subTotal": 0,

"tax": 0.00,

"total": 1,

"fee": 0.6000000238418579,

"netAmount": 0.40,

"totalRefundedAmount": 0,

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": 1,

"price": 10,

"tax": 0,

"metadata": "metadata"

}

],

"isNonProfit": false

}

}

```
## Transaction Pending to be confirmed by the ATH Móvil customer (/payment) Response: Status **OPEN**
```

{

    "status": "success",

    "data": {

        "ecommerceStatus": "OPEN",

        "ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",

        "referenceNumber": "",

        "businessCustomerId": "402894d56e713892016e7f2963de0010",

        "transactionDate": "",

        "dailyTransactionId": "",

        "businessName": "Tdameritrade",

        "businessPath": "Tdameritrade",

        "industry": "COMPUTERS",

        "subTotal": 1.33,

        "tax": 1.00,

        "total": 2.33,

        "fee": 0.00,

        "netAmount": 0,

        "totalRefundedAmount": 0,

        "metadata1": "Metadata 1",

        "metadata2": "Metada 2",

        "items": [

            {

                "name": "Diego MO",

                "description": "Diego",

                "quantity": 1,

                "price": 1.33,

                "tax": 1,

                "metadata": "metadata,

                "formattedPrice": "",

                "sku": ""

            }

        ],

        "isNonProfit": false

    }

```

## Transaction confirmed by the ATH Móvil customer but pending to be processed by the merchant (/payment+/confirm) Response: Status **CONFIRM**
```

{

    "status": "success",

    "data": {

        "ecommerceStatus": "CONFIRM",

        "ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",

        "referenceNumber": "",

        "businessCustomerId": "402894d56e713892016e7f2963de0010",

        "transactionDate": "",

        "dailyTransactionId": "",

        "businessName": "Tdameritrade",

        "businessPath": "Tdameritrade",

        "industry": "COMPUTERS",

        "subTotal": 1.33,

        "tax": 1.00,

        "total": 2.33,

        "fee": 0.00,

        "netAmount": 0,

        "totalRefundedAmount": 0,

        "metadata1": "Metadata 1",

        "metadata2": "Metada 2",

        "items": [

            {

                "name": "Diego MO",

                "description": "Diego",

                "quantity": 1,

                "price": 1.33,

                "tax": 1,

                "metadata": "metadata",

                "formattedPrice": "",

                "sku": ""

            }

        ],

        "isNonProfit": false

    }

}

```
## Transaction Expired or Canceled Response: Status CANCEL
```

{

    "status": "success",

    "data": {

        "ecommerceStatus": "CANCEL",

        "ecommerceId": "29bc7846-e44f-11ed-b127-839ef0792c17",

        "referenceNumber": "",

        "businessCustomerId": "402894d56e713892016e7f2963de0010",

        "transactionDate": "",

        "dailyTransactionId": "",

        "businessName": "Tdameritrade",

        "businessPath": "Tdameritrade",

        "industry": "COMPUTERS",

        "subTotal": 1.33,

        "tax": 1.00,

        "total": 2.33,

        "fee": 0.00,

        "netAmount": 0,

        "totalRefundedAmount": 0,

        "metadata1": "Metadata 1",

        "metadata2": "Metada 2",

        "items": [

            {

                "name": "Diego MO",

                "description": "Diego",

                "quantity": 1,

                "price": 1.33,

                "tax": 1,

                "metadata": "metadata",

                "formattedPrice": "",

                "sku": ""

            }

        ],

        "isNonProfit": false

    }

}

```
## Authorization Service

![Shape6](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

After the user confirms the transaction, the ecommerceID gets updated with the confirmation of the user. The last step is to run the authorization services. This service takes the ticket and authorization of the customer and process the transaction in our core system debiting the funds of the customer.

This service "/authorization" requires a payload with one mandatory attribute "ecommerceId", which will be validated by the same service. This service call must be authenticated with JWT.

**Endpoint:** https://payments.athmovil.com/api/business-transaction/ecommerce/authorization

**Headers:** Content-type – application/json

**Request**

- **ecommerceId** : This ID represent the ticket of the transaction to be paid with the information provided in the request.

curl --location --request POST 'HOST/business-transaction/ecommerce/authorization \

--header 'Authorization: Bearer TOKEN' \

--header 'Accept: application/json' \

--header 'Authorization: Bearer \

--header 'Content-Type: application/json' \

--data-raw 

```
{

    "ecommerceId": "41945f05-2b1a-11ed-a1c5-077060cc68b2"

}

```

**Response**

- **status** : Confirm status of the service response.
- **ecommerceStatus** : represents the status of the ecommerce transaction, either completed, cancelled or expired.
- **ecommerceID:** Represents the transaction ticket ID token
- **referenceNumber:** Unique transaction identifier.
- **transactionDate** : Authorization date
- **dailyTransactionID** : ID count for the transaction in the day.
- **businessName** : Business Name for the ATH Business account
- **businessPath** : Business Path for the ATH Business account
- **industry** : Industry of the business
- **total** : Total amount
- **tax** : Tax captured in the transaction.
- **subtotal** : Subtotal of the transaction
- **fee** : Fee charge by ATH Business
- **netAmount** : Net amount to be funded to the ATH Business account
- **totalRefundedAmount** : Total refunded amount of the original transaction
- **metadata1** : variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- **metadata2** : variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- **items** : Items paid for in the transaction.

```

{

"status": "success",

"data": {

"ecommerceStatus": "COMPLETED",

"ecommerceId": "730e2c49-9387-11ed-8f43-c31784ccfc6c",

"referenceNumber": "215070443-402894c185ab1be40185acfe61c2000b",

"businessCustomerId": "402894d56e713892016e7f2963de0010",

"transactionDate": "2023-01-13 16:17:06",

"dailyTransactionId": "0006",

"businessName": "Tdameritrade",

"businessPath": "Tdameritrade",

"industry": "ENTERTAINMENT",

"subTotal": 0,

"tax": 0.00,

"total": 1,

"fee": 0.6000000238418579,

"netAmount": 0.40,

"totalRefundedAmount": 0,

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": 1,

"price": 10,

"tax": 0,

"metadata": "Articulo 1"

}

],

"isNonProfit": false

}

}

```


## Update Payment (Phone Number)

![Shape8](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

This is a Web Service that will allow the merchant to update the phone number. The updated phone number will receive the push notification to confirm the payment in the ATH Móvil's app.

**Endpoint:** https://payments.athmovil.com/api/business-transaction/ecommerce/business/updatePhoneNumber

**Headers:** Content-type – application/json

**Request**

**ecommerceId** : This ID represent the ticket of the transaction to be paid with the information provided in the request.

**phoneNumber** : Determines the phone number registered in the business account that the payment will be sent to.

Type: REST

Format: application/json

Method: PUT

Header:

    - Accept: application/json
    - Content-Type: application/json
    - Authorization; Bearer Token
    - Host
    - Required Auth Token of type Bearer

"ecommerceId": "177a50fd-39fb-11ed-8b3d-230262020527",

"phoneNumber": "7866575255",

}'

**Response:**
```

{

"status": "success",

"data": "Update Phone Number"

}

```
## Refund Payment

![Shape9](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

This is a Web Service that allows to refund a completed ecommerce transaction.

- Business Transaction
  - Endpoint: "https://payments.athmovil.com/api/business-transaction/ecommerce/refund"
  - Object Request: {"amount": 0, "message": "string", "privateToken": "string", "publicToken": "string", "referenceNumber": "string"}
  - Type: REST
  - Format: application/json
  - Method: POST
  - Header:
    - Accept: application/json
    - Content-Type: application/json
    - Authorization; Bearer Token
    - Host

**Request:**
```

{

"publicToken": "hdb932832klnasKJGDW90291",

"privateToken": "JHEFEWP2048FNDFLKJWB2",

"referenceNumber": "fdew98ffw9fbfewkjb", //transactionId

"amount": "1.00",

"message": "MSG Test" //Optional

}

```

**Response**
```

{

"status": "success",

"data": {

"refund": {

"transactionType": "REFUND",

"status": "COMPLETED",

"refundedAmount": 1.00,

"date": "123412341234", //timestamp

"referenceNumber": "402894d56b240610016b2e6c78a6003a", //Refund transactionId

"dailyTransactionID": "0107",

"name": " Valeria Herrero",

"phoneNumber": "(787) 123-4567",

"email": "valher@gmail.com"

},

"originalTransaction": {

"transactionType": "PAYMENT",

"status": "COMPLETED",

"date": "123412341234", //timestamp

"referenceNumber": "402894d56b240610016b2e6c78a6003a", //Original Payment transactionId

"dailyTransactionID": "0106",

"name": "Valeria Herrero",

"phoneNumber": "(787) 123-4567",

"email": "valher@gmail.com",

"message": "",

"total": 1.00,

"tax": 0.00,

"subtotal": 0.00,

"fee": 0.00,

"netAmount": 0.00,

"totalRefundedAmount": 1.00,

"metadata1": "metadata1 test",

"metadata2": "metadata2 test",

"items": [

]

}

```

## Cancel Payment

![Shape10](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

This is a Web Service to cancel the ecommerce transaction.

- Business Transaction
  - Endpoint: https://payments.athmovil.com/api/business-transaction/ecommerce/business/cancel"
  - Object Request: {"ecommerceId", "publicToken "
  - Type: REST
  - Format: application/json
  - Method: POST
  - Header:
    - Accept: application/json
    - Content-Type: application/json
    - Authorization; Bearer Token
    - Host

**Request:**
```

"ecommerceId": "177a50fd-39fb-11ed-8b3d-230262020527",

"publicToken": "3adc528b182e50b41acff658136bd974c89604c3"

```
**Response:**
```

{

    "status": "success",

    "data": "Payment Cancelled."

}

```
#

## Error Messages

![Shape11](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

The error messages for the PB API's will follow a standard response structure of status, message, error code and data. Below are some examples of the responses and a list of all available error scenarios.

## Request without a token authentication.
```

{

    "status": "error",

    "message": "No authorization header present.",

    "errorcode": "token.invalid.header",

    "data": null

}

```
## Request with an expired token.
```

{

    "status": "error",

    "message": "The Token has expired on Fri Oct 28 15:21:00 AST 2022.",

    "errorcode": "token.expired",

    "data": null

}

```
## When the Object request it's empty.
```

{

    "status": "error",

    "message": "Required request body is missing",

    "errorcode": "BTRA\_0006",

    "data": null

}

```
**Additional Error Codes** :

| Error code | Description |
| --- | --- |
| error.code.es.BTRA\_9998 | =Communication Error |
| error.code.en.BTRA\_9998 | =Communication Error |
| error.code.es.BTRA\_9999 | =Internal Error |
| error.code.en.BTRA\_9999 | =Internal Error |
| error.code.es.BTRA\_0401 | =Invalid authorization token |
| error.code.en.BTRA\_0401 | =Invalid authorization token |
| error.code.es.BTRA\_0402 | =Authorization token expired |
| error.code.en.BTRA\_0402 | =Authorization token expired |
| error.code.es.BTRA\_0403 | =Invalid authorization token |
| error.code.en.BTRA\_0403 | =Invalid authorization token |
| error.code.es.BTRA\_0001 | =El monto de la transferencia está por debajo del mínimo permitido |
| error.code.en.BTRA\_0001 | =The transfer amount is under Minimum allowed |
| error.code.es.BTRA\_0002 | =Estatus del cliente es Pendiente Recuperar Verificación de Acceso |
| error.code.en.BTRA\_0002 | =Customer status is Pending Regain Access Verification |
| error.code.es.BTRA\_0003 | =Número de tarjeta del cliente es el mismo que el del negocio |
| error.code.en.BTRA\_0003 | =Card number from the customer is the same than the business |
| error.code.es.BTRA\_0004 | =El monto está por encima de los límites |
| error.code.en.BTRA\_0004 | =Amount is Over the limits |
| error.code.es.BTRA\_0005 | =Transacción fallida. Estatus de error |
| error.code.en.BTRA\_0005 | =Transaction failed. Status error |
| error.code.es.BTRA\_0006 | =Formato inválido |
| error.code.en.BTRA\_0006 | =Invalid format |
| error.code.es.BTRA\_0007 | =TransactionId no existe |
| error.code.en.BTRA\_0007 | =TransactionId does not exist |
| error.code.es.BTRA\_0008 | =TransactionId no pertenece al negocio |
| error.code.en.BTRA\_0008 | =TransactionId does not belong to the business |
| error.code.es.BTRA\_0009 | =El negocio no está activo |
| error.code.en.BTRA\_0009 | =Business is not active |
| error.code.es.BTRA\_0010 | =El negocio no está activo |
| error.code.en.BTRA\_0010 | =Business status is not Active |
| error.code.es.BTRA\_0011 | =Número de la tarjeta no pertenece al negocio |
| error.code.en.BTRA\_0011 | =Card number does not belong to the business |
| error.code.es.BTRA\_0012 | =Números de tarjetas origen y destino son los mismos |
| error.code.en.BTRA\_0012 | =Source and Target card numbers are the same |
| error.code.es.BTRA\_0013 | =El monto es cero |
| error.code.en.BTRA\_0013 | =Amount is Zero |
| error.code.es.BTRA\_0014 | =TransactionId no existe para el negocio actual |
| error.code.en.BTRA\_0014 | =TransactionId does not exist for the current business |
| error.code.es.BTRA\_0015 | =Communication Error |
| error.code.en.BTRA\_0015 | =Communication Error |
| error.code.es.BTRA\_0016 | =Communication Error |
| error.code.en.BTRA\_0016 | =Communication Error |
| error.code.es.BTRA\_0017 | =Invalid authorization token |
| error.code.en.BTRA\_0017 | =Invalid authorization token |
| error.code.es.BTRA\_0018 | =no debe estar vacío |
| error.code.en.BTRA\_0018 | =must not be blank |
| error.code.es.BTRA\_0019 | =no debe ser nulo |
| error.code.en.BTRA\_0019 | =must not be null |
| error.code.es.BTRA\_0020 | =valor numérico fuera de los límites (\<4 dígitos\>. \<2 dígitos\> esperados) |
| error.code.en.BTRA\_0020 | =numeric value out of bounds (\<4 digits\>.\<2 digits\> expected) |
| error.code.es.BTRA\_0021 | =TransactionType no es válido |
| error.code.en.BTRA\_0021 | =TransactionType is not valid |
| error.code.es.BTRA\_0022 | =Reporte sin transacciones |
| error.code.en.BTRA\_0022 | =Not record for report |
| error.code.es.BTRA\_0023 | =Error con la fecha Desde |
| error.code.en.BTRA\_0023 | =Error with the From date |
| error.code.es.BTRA\_0024 | =Error con la fecha Hasta |
| error.code.en.BTRA\_0024 | =Error with the To date |
| error.code.es.BTRA\_0025 | =La fecha Desde debe ser anterior a la fecha Hasta |
| error.code.en.BTRA\_0025 | =The From date must be before the To date |
| error.code.es.BTRA\_0026 | =La diferencia entre fechas debe ser inferior a %s días |
| error.code.en.BTRA\_0026 | =The difference between dates must be lower than %s days |
| error.code.es.BTRA\_0027 | =La organización no es una organización sin fines de lucro |
| error.code.en.BTRA\_0027 | =The organization is not a Non Profit organization |
| error.code.es.BTRA\_0028 | =es menor de 0.01 |
| error.code.en.BTRA\_0028 | =is less than 0.01 |
| error.code.es.BTRA\_0029 | =El parámetro requerido '%s' no está presente |
| error.code.en.BTRA\_0029 | =Required String parameter '%s' is not present |
| error.code.es.BTRA\_0030 | =El refund fallo con un estatus de error |
| error.code.en.BTRA\_0030 | =The refund failed with a status error |
| error.code.es.BTRA\_0031 | =El ecommerceId no existe |
| error.code.en.BTRA\_0031 | =EcommerceId does not exist |
| error.code.es.BTRA\_0032 | =El estatus del e-commerce no esta confirmado |
| error.code.en.BTRA\_0032 | =The status of the e-commerce is not confirm |
| error.code.es.BTRA\_0033 | =Error al crear el registro del BusinessEcommerce |
| error.code.en.BTRA\_0033 | =Error creating BusinessEcommerce recordb |
| error.code.es.BTRA\_0034 | =El BusinessEcommerce ya existe en dynamoDB |
| error.code.en.BTRA\_0034 | =BusinessEcommerce exists in the dynamoDB |
| error.code.es.BTRA\_0035 | =El estatus del registro de Transacción e-commerce no es el espereado |
| error.code.en.BTRA\_0035 | =The e-commerce transaction has not the expected status |
| error.code.es.BTRA\_0036 | =El e-commerce que tratas de confirmar, ya esta asignado a otro customer |
| error.code.en.BTRA\_0036 | =The e-commerce you are trying to confirm is already assigned to another customer |
| error.code.es.BTRA\_0037 | =No se puede confirmar una transacción con status Cancelado o Fallido |
| error.code.en.BTRA\_0037 | =Cannot confirm a transaction with status Canceled or Failed |
| error.code.es.BTRA\_0038 | =El campo metadata excede el maximo de caracteres soportado (40) |
| error.code.en.BTRA\_0038 | =The metadata field exceeds the maximum supported characters (40) |
| error.code.es.BTRA\_0039 | =Tiempo valido para ejecutar la transacción ha expirado |
| error.code.en.BTRA\_0039 | =Valid time to execute the transaction has expired |
| error.code.es.BTRA\_0040 | =El mensaje no puede superar los 50 caracteres |
| error.code.en.BTRA\_0040 | =The message can not exceed 50 characters |
| error.code.es.BTRA\_0041 | =La transaccion e-commerce ya tiene asignado un numero telefonico |
| error.code.en.BTRA\_0041 | =The e-commerce transaction already has a phone number assigned |
| error.code.es.BTRA\_0042 | =El monto de la propina está por encima de los límites |
| error.code.en.BTRA\_0042 | =Tip amount is over limits |
| error.code.es.BTRA\_0043 | =El comercio no es dueño del pago |
| error.code.en.BTRA\_0043 | =The business ins't owner of payment |
| error.code.es.BTRA\_0044 | =El monto está por encima de los límites del Institución Financiera |
| error.code.en.BTRA\_0044 | =Amount is Over limits of the Financial Institution |
| error.code.es.BTRA\_0045 | =El monto está por encima de los límites de la tarjeta |
| error.code.en.BTRA\_0045 | =Amount is Over the limits of the card |
| error.code.es.BTRA\_0046 | =No hay transacciones para el rango de fechas indicado |
| error.code.en.BTRA\_0046 | =There are no transactions for the indicated date range |
| error.code.es.BTRA\_0047 | =La fecha desde no puede ser futura |
| error.code.en.BTRA\_0047 | =Date from cannot be future |
| error.code.es.BTRA\_0048 | =La fecha hasta no puede ser futura |
| error.code.en.BTRA\_0048 | =Date to cannot be future |
| error.code.es.BTRA\_0049 | =El customerId no existe |
| error.code.en.BTRA\_0049 | =customerId does not exist |
| error.code.es.BTRA\_0050 | =La configuracion para recibir propinas esta deshabilitada para el negocio |
| error.code.en.BTRA\_0050 | =Tip configuration is disabled for the business |
| error.code.es.BTRA\_0051 | =El monto de la propina no debe exceder el valor del monto de la transaccion |
| error.code.en.BTRA\_0051 | =The amount of the tip must not exceed the value of the amount of the transaction |
| error.code.es.BTRA\_0052 | =No puede llenar ambos campos (tipAmount y percentage), solo use uno |
| error.code.en.BTRA\_0052 | =Cannot fill both fields (tipAmount and percentage), just fill one |
| error.code.es.BTRA\_0053 | =El ReferenceNumber no existe |
| error.code.en.BTRA\_0053 | =ReferenceNumber not found |

## Reporting

![Shape12](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

Withing the reports of ATH Business this transaction will be registered as an ECOMMERCE type transaction. Transactions performed through the "Pay a Business" functionality within the ATHM app will register as PAYMENT type transactions.

Reporting example

<img width="468" alt="report" src="https://github.com/evertec/ATHM-Payment-Button-API/assets/99409598/8080c8e9-5afe-4d49-b199-7f80582381b0">

## Other information

![Shape13](RackMultipart20230620-1-9lu3np_html_b18e7627adeba20.gif)

Additionally, if you only want to receive payments through the payment button, we recommend disabling the business from the list of business available in the ATH Móvil's app.

This way you guarantee you are receiving payments only through the payment button.

This change can be performed within the app via the settings.

<img width="468" alt="Picture2" src="https://github.com/evertec/ATHM-Payment-Button-API/assets/99409598/221d4b6f-b3c2-4630-8319-e86736eb0dc2">


# Ether Private Bank API Documentation

## Overview

This API allows clients of Ether Private Bank to authenticate using their email and password, and interact with various services like generating dynamic BR Codes, making payments, and checking wallet balances.

## Authentication
The authentication process involves logging in with email and password. On successful login, a JWT (JSON Web Token) is returned, which is valid for 365 days. This token is used to authenticate subsequent requests.

## Postman Collection Details
- **Collection Name:** Betnovinha
- **Base URL Variable:** `{{url}}`
- **Authentication Type:** Bearer Token

### Required Environment Variables

| Variable Name | Description                        | Example Value                               |
|---------------|------------------------------------|---------------------------------------------|
| `url`         | Base URL for API requests          | `https://api.etherprivatebank.com.br`       |
| `c_email`     | Client’s email address             | `example@domain.com`                        |
| `c_password`  | Client’s password                  | `your_password`                             |
| `c_token`     | JWT token (automatically set)      |                                             |
| `c_userId`    | User ID (automatically set)        |                                             |
| `brcodeId`    | BR Code ID (automatically set)     |                                             |
| `uuid`        | UUID for BR Code (automatically set) |                                             |

## Endpoints

### 1. POST /auth/login
**Description:**  
This endpoint is used to authenticate the client using their email and password. It returns a JWT token that is valid for 365 days, which should be used in subsequent requests.

- **Method:** `POST`
- **URL:** `{{url}}/auth/login`
- **Headers:**
  - `Content-Type: application/json`
  - `Referer: https://app.etherprivatebank.com.br/`
- **Request Body (JSON):**
\```json
{
    "email": "{{c_email}}",
    "password": "{{c_password}}"
}
\```
- **Response:**
  - The response includes `access_token` and `userId` which are automatically stored in the environment variables `c_token` and `c_userId`.
- **Tests:**
  - Verifies the presence of `access_token` and `userId`.
  - Automatically sets the token and userId for future requests.

### 2. POST /brcodes/v3 (Create Dynamic BR Code)
**Description:**  
This endpoint creates a dynamic BR Code for payment.

- **Method:** `POST`
- **URL:** `{{url}}/brcodes/v3`
- **Headers:**
  - `Content-Type: application/json`
  - `Accept: application/json`
- **Request Body (JSON):**
\```json
{
    "amount": 100
}
\```
- **Response:**
  - Returns the `brcodeId` and `uuid` which are automatically stored in the environment variables.
- **Tests:**
  - Verifies if the response status code is `201`.
  - Ensures that the `brcodeId` and `uuid` are set correctly.

### 3. POST /brcodes/pay (Make a BR Code Payment)
**Description:**  
This endpoint allows clients to make a payment using a BR Code.

- **Method:** `POST`
- **URL:** `{{url}}/brcodes/pay`
- **Headers:**
  - `Content-Type: application/json`
  - `Accept: application/json`
- **Request Body (JSON):**
\```json
{
    "brcode": "00020126870014br.gov.bcb.pix0136fb29e744-f35e-48d3-8659-be44aa533f18022516K4f1df2bb0d984d6ab2354d52040000530398654041.005802BR5918ETHER PRIVATE BANK6009SAO PAULO6229052516K4f1df2bb0d984d6ab2354d630430C8"
}
\```

### 4. GET /wallets/ebrl-balance/{c_userId} (Check Balance)
**Description:**  
This endpoint retrieves the wallet balance for the authenticated user.

- **Method:** `GET`
- **URL:** `{{url}}/wallets/ebrl-balance/{{c_userId}}`
- **Headers:** None required

## Webhook: Handling Deposits

When a user deposits money into their account, the API sends a webhook notification. Below is an example of the JSON payload that you can expect to receive:

### Example Webhook Payload for a Deposit

```json
{
  "event": {
    "created": "2024-08-28T19:24:01.012648+00:00",
    "id": "5356852523040768",
    "log": {
      "created": "2024-08-28T19:24:00.500812+00:00",
      "deposit": {
        "accountNumber": "6219417801195520",
        "accountType": "payment",
        "amount": 10005,
        "bankCode": "20018183",
        "branchCode": "0001",
        "created": "2024-08-28T19:23:59.396307+00:00",
        "displayDescription": "",
        "fee": 0,
        "id": "5160583015956480",
        "name": "Mock Payment Institution Ltd",
        "status": "created",
        "tags": [
          "e20018183202408281923xg35f29fxuz",
          "dynamic-brcode/b61d1db4165e4ba290b3a04f6ca5e3f2",
          "47544a724041488",
          "db61d1db4165e4ba290b3a04f6ca5e3f2"
        ],
        "taxId": "12.345.678/0001-90",
        "transactionIds": [
          "30244242259666266132291935478651"
        ],
        "type": "pix",
        "updated": "2024-08-28T19:24:00.500886+00:00"
      },
      "errors": [],
      "id": "4953260347621376",
      "type": "credited"
    },
    "subscription": "deposit",
    "workspaceId": "6219417801195520"
  }
}
```

## Authentication Details

- Once authenticated, the `access_token` is automatically stored in the environment as `c_token`.
- This token must be sent as a Bearer token in the Authorization header for all authenticated requests.

## How to Use the Postman Collection

1. **Set Up Environment Variables:**
   - Enter your base URL (`url`), email (`c_email`), and password (`c_password`) in the environment settings.

2. **Authenticate:**
   - Run the **POST /auth/login** request to retrieve and set your token (`c_token`) and user ID (`c_userId`).

3. **Perform Operations:**
   - Use the other endpoints as needed, ensuring that the token is included in the requests.

4. **Testing:**
   - The Postman collection automatically tests key responses like token retrieval and BR Code creation.

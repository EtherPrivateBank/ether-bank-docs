Ether Private Bank API Documentation

Overview

This API allows clients of Ether Private Bank to authenticate using their email and password, and interact with various services like generating dynamic BR Codes, making payments, and checking wallet balances.

Authentication
The authentication process involves logging in with email and password. On successful login, a JWT (JSON Web Token) is returned, which is valid for 365 days. This token is used to authenticate subsequent requests.

Postman Collection Details
Collection Name: Betnovinha
Base URL Variable: {{url}}
Authentication Type: Bearer Token

Required Environment Variables
Variable Name	Description	Example Value
url	Base URL for API requests	https://api.etherprivatebank.com.br
c_email	Client’s email address	example@domain.com
c_password	Client’s password	your_password
c_token	JWT token (automatically set)	
c_userId	User ID (automatically set)	
brcodeId	BR Code ID (automatically set)	
uuid	UUID for BR Code (automatically set)	

Endpoints

1. POST /auth/login
Description:
This endpoint is used to authenticate the client using their email and password. It returns a JWT token that is valid for 365 days, which should be used in subsequent requests.

Method: POST
URL: {{url}}/auth/login
Headers:
Content-Type: application/json
Referer: https://app.etherprivatebank.com.br/
Request Body (JSON):
{
    "email": "{{c_email}}",
    "password": "{{c_password}}"
}
Response:
The response includes access_token and userId which are automatically stored in the environment variables c_token and c_userId.
Tests:
Verifies the presence of access_token and userId.
Automatically sets the token and userId for future requests.

2. POST /brcodes/v3 (Create Dynamic BR Code)
Description:
This endpoint creates a dynamic BR Code for payment.

Method: POST
URL: {{url}}/brcodes/v3
Headers:
Content-Type: application/json
Accept: application/json
Request Body (JSON):
{
    "amount": 100
}
Response:
Returns the brcodeId and uuid which are automatically stored in the environment variables.
Tests:
Verifies if the response status code is 201.
Ensures that the brcodeId and uuid are set correctly.

3. POST /brcodes/pay (Make a BR Code Payment)
Description:
This endpoint allows clients to make a payment using a BR Code.

Method: POST
URL: {{url}}/brcodes/pay
Headers:
Content-Type: application/json
Accept: application/json
Request Body (JSON):
{
    "brcode": "00020126870014br.gov.bcb.pix0136fb29e744-f35e-48d3-8659-be44aa533f18022516K4f1df2bb0d984d6ab2354d52040000530398654041.005802BR5918ETHER PRIVATE BANK6009SAO PAULO6229052516K4f1df2bb0d984d6ab2354d630430C8"
}

4. GET /wallets/ebrl-balance/{c_userId} (Check Balance)
Description:
This endpoint retrieves the wallet balance for the authenticated user.

Method: GET
URL: {{url}}/wallets/ebrl-balance/{{c_userId}}
Headers: None required

Authentication Details

Once authenticated, the access_token is automatically stored in the environment as c_token.
This token must be sent as a Bearer token in the Authorization header for all authenticated requests.

How to Use the Postman Collection

Set Up Environment Variables:
Enter your base URL (url), email (c_email), and password (c_password) in the environment settings.

Authenticate:
Run the POST /auth/login request to retrieve and set your token (c_token) and user ID (c_userId).

Perform Operations:
Use the other endpoints as needed, ensuring that the token is included in the requests.

Testing:
The Postman collection automatically tests key responses like token retrieval and BR Code creation.

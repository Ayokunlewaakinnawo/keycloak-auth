# Keycloak API Documentation

## Description

This is an (API) that is specifically designed to handle user login procedures. It works by carrying out a series of authentication checks to verify the identity of the user. This process is crucial in ensuring the security and integrity of the user's data. 

**ENDPOINTS:**

- Generate Access Token / Authenticate User.
- Logout
- Create/Register User
- Introspect User Token
- Refresh User Access Token

## Base URL

The base URL for all API requests is:

`https://localhost:8080`

## 1. Endpoints

### **Generate Access Token / Authenticate User**.

- **URL:** `**{Base_URL}/realms/vox-service/protocol/openid-connect/token**`
- **Method:** POST
- **Description:**This HTTP POST request is used to obtain an access token by providing client credentials and user authentication. The request should be sent to [t](http://0.0.0.0:8080/realms/vox-service/protocol/openid-connect/token)he URL with the x-www-form-urlencoded request body type.
- **Request Body:**

| field | type | Description |
| --- | --- | --- |
| client_id | string  |  The client ID for authentication. |
| client_secret | string | The client secret for authentication. |
| grant_type | string | The type of grant being requested. |
| username | string | The username for user authentication. |
| password | string | The password for user authentication. |

```json
{
  "client_id": "admin2.0",
  "client_secret": "oVnLGLO8qUXYiPcEbodS7UdkvYu05UGU",
	"grant_type": "password"
	"username": "user_name",
  "password": "user_password",
}
```

### **Success Response:**

- **Code:** 200 OK

Upon successful execution, the response will have a status code of 200 and a content type of application/json. The response body will include the following parameters:

- **Example Response Content:**

```json
{
    "**access_token**": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJjakxreW9VS2FqTVg1Z2dGdVI2OGt5WDZWakJEUFc1UEJ0YzJrN2tlZkVzIn0.eyJleHAiOjE3MDc0MDI0NDcsImlhdCI6MTcwNzQwMjE0NywianR...",
    "**expires_in**": 300,
    "**refresh_expires_in**": 1800,
    "**refresh_token**": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJmZTY1YjJjMC0zY2NmLTQ3YTgtODlmMi05MWQzZm...",
    "**token_type**": "Bearer",
    "**not-before-policy**": 0,
    "session_state": "abacb845-3531-4239-b770-1dc6e6ec0e48",
    "scope": "email profile"
}
```

- `access_token`: The obtained access token.
- `expires_in`: The duration until the access token expires.
- `refresh_expires_in`: The duration until the refresh token expires.
- `refresh_token`: The refresh token to obtain a new access token.
- `token_type`: The type of token obtained.
- `not-before-policy`: A policy indicating when the token is valid.
- `session_state`: The state of the user's session.
- `scope`: The scope of the access token.

## 2. Endpoints

### Logout

- **URL:** **`{Base_URL}/realms/vox-service/protocol/openid-connect/logout`**
- **Method:** POST
- **Description:**  This endpoint is used to log out a user from the OpenID Connect protocol, with the x-www-form-urlencoded request body type.
- **Request Body:**

```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldU...",
  "client_id": "admin2.0",
	"client_secret": "oVnLGLO8qUXYiPcEbodS7UdkvYu05UGU"
}
```

| field | type | Description |
| --- | --- | --- |
| refresh_token | string  | refresh_token generated from authenticating user |
| client_id | string | The client ID registered in Keycloak. |
| client_secret | string | The client secret associated with the client ID.  |

### **Success Response:**

- **Code:** 204 No Content

## 3. Endpoints

## Create/Register User

The initial aspect in registration of creating a new user is generating an admin access token that is then used for authorization in registering a user API endpoint;

### GET ADMIN TOKEN

This API gets the admin access token that would aid a user self-register.

- **URL:** **`{Base_URL}/realms/master/protocol/openid-connect/token`**
- **Method:** POST
- **Description:** This HTTP POST request is used to obtain an access token by providing client credentials. The request should be sent to the specified URL with the x-www-form-urlencoded request body type. The request body should include the following parameters:
- **Request Body:**
    
    ```json
    {
    "client_id": "admin-cli"
    
    "client_secret": "E0jWBP0GvHmxGl9FEgOBxqN8UNoW03ZR"
    
    "grant_type": "client_credentials"
    }
    ```
    

| field | type | Description |  |
| --- | --- | --- | --- |
| client_id | string  | client id of master’s realm admin-cli | The client ID used for authentication. |
| client_secret | string | client secret of admin-cli configured from the AC | The client secret used for authentication. |
| grant_type | string | grant type configured from the admin console; | The type of grant being requested. |

### **Success Response:**

The response to this request will have a status code of 200 and a content type of application/json. The response body will include the following parameters:

- **Example Content:**

```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJEQjBtbXpxeG50WnVYUGl1eDI4UUFqQVBuc1Q4am9aRW9kNEhhRTR0N0JvIn0.eyJleHAiOjE3MDg1Mzc5MjAsImlhdCI6MTcwODUzNzg2MCwianRpIjoiYzcxMGQ....",
    "expires_in": 60,
    "refresh_expires_in": 0,
    "token_type": "Bearer",
    "not-before-policy": 0,
    "scope": "email profile"
}
```

**Content Description:**

- `access_token`: The obtained access token.
- `expires_in`: The duration in seconds for which the access token is valid.
- `refresh_expires_in`: The duration in seconds for which the refresh token is valid.
- `token_type`: The type of token obtained.
- `not-before-policy`: The time before which the token cannot be used.
- `scope`: The scope of the access token.

### CREATE USER

This endpoint allows you to create a new user within the "vox-service" realm under the "admin" category. The request should be sent as an HTTP POST to the specified URL.

- **URL:** **`{base_url}/admin/realms/vox-service/users`**
- **Method:** POST
- **Description:** This uses the token generated by the admin (master) to authenticate the creation of a user.
- **Request Body:**

The request body should be in raw format and include the following parameters:

| field | type | Description |
| --- | --- | --- |
| enabled | boolean  |  Indicates whether the user is enabled. |
| username | string |  The first name of the user. |
| lastName | string | The last name of the user. |
| email | string | The email address of the user. |
| credentials | array | An array containing the user's credentials, with each credential object having the following properties: |
| temporary | boolean |  Indicates whether the credential is temporary. |
| type | string | The type of the credential. |
| value | string | The value of the credential. |

Request Body Example:

```json
{ 
    "enabled":true,
    "username":"aakinn",
    "firstName":"ayokunlwa",
    "lastName":"akinn",
    "email":"aakinnnawo@test.fr",
    "credentials":[
        {
            "temporary": false,
            "type":"password",
            "value":"mypassword"
        }
    ]
}
```

### **Token:**

**<token> : admin-cli `access_token`** generated earlier

### **Authorization:**

Bearer Token

### **Success Response:**

Upon a successful execution, the endpoint returns a status code of 201 and the content type of the response is "text/xml". The response body is not available (null).

### Important Note:

Keycloak provides a default UI for users to self register and login into the default account profile info 

- **URL:`{Base_URL}/realms/vox-service/account/`**

## 4. Endpoints

### Introspect Token

This API allows clients to introspect (verify and obtain details about) a token issued by Keycloak.

- **URL:`{Base_URL}/realms/vox-service/protocol/openid-connect/token/introspect`**
- **Method:** POST
- **Description:** verify its validity and obtain token details, with the x-www-form-urlencoded request body type.
- **Request Body:**
    
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldU...",
      "client_id": "admin2.0",
    	"client_secret": "oVnLGLO8qUXYiPcEbodS7UdkvYu05UGU"
    }
    ```
    

| field | type | Description |
| --- | --- | --- |
| token | string  | access_token  generated in Keycloak. |
| client_id | string | The client ID registered in Keycloak. |
| client_secret | string | The client secret associated with the client ID.  |

### **Success Response:**

Upon successful code 200 execution, the API returns a JSON object with the following fields:

- `exp`: The expiration time of the token.
- `iat`: The time at which the token was issued.
- `jti`: The unique identifier for the token.
- `iss`: The issuer of the token.
- `aud`: The audience for which the token is intended.
- `sub`: The subject of the token.
- `typ`: The token type.
- `azp`: The authorized party.
- `session_state`: The session state.
- `acr`: The authentication context class reference.
- `allowed-origins`: An array of allowed origins.
- `realm_access`: Details about roles within the realm.
- `resource_access`: Details about roles for specific resources.
- `scope`: The scope of the token.
- `sid`: The session ID.
- `email_verified`: Indicates if the email is verified.
- `name`: The name associated with the token.
- `preferred_username`: The preferred username.
- `given_name`: The given name.
- `family_name`: The family name.
- `email`: The email associated with the token.
- `client_id`: The client identifier.
- `username`: The username.
- `token_type`: The type of token.
- `active`: Indicates if the token is active.
- **Example of Content:**
    
    ```json
    {
        "exp": 1707404676,
        "iat": 1707404376,
        "jti": "17a2361b-dde5-4199-a287-84ad770b1896",
        "iss": "http://0.0.0.0:8080/realms/VOX-SERVICES",
        "aud": "account",
        "sub": "b87faea3-b486-4376-ad19-f578df8dfce7",
        "typ": "Bearer",
        "azp": "Admin-2.0",
        "session_state": "15aef8e2-fe53-42db-ad66-1e5f7168c059",
        "acr": "1",
        "allowed-origins": [
            "*"
        ],
        "realm_access": {
            "roles": [
                "offline_access",
                "default-roles-vox-services",
                "uma_authorization"
            ]
        },
        "resource_access": {
            "account": {
                "roles": [
                    "manage-account",
                    "manage-account-links",
                    "view-profile"
                ]
            }
        },
        "scope": "email profile",
        "sid": "15aef8e2-fe53-42db-ad66-1e5f7168c059",
        "email_verified": true,
        "name": "Ayo Akinnawo",
        "preferred_username": "ayo",
        "given_name": "Ayo",
        "family_name": "Akinnawo",
        "email": "aakinnawo@voxtechnologies.com",
        "client_id": "Admin-2.0",
        "username": "ayo",
        "token_type": "Bearer",
        "active": true
    }
    ```
    

## 5. Endpoints

### Refresh Access Token

This endpoint is used to obtain an access token by exchanging a refresh token

- **URL:`{Base_URL}/realms/vox-service/protocol/openid-connect/token`**
- **Method:** POST
- **Description:** verify its validity and obtain token details, with the x-www-form-urlencoded request body type.
- **Request Body:**
    
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldU...",
      "client_id": "admin2.0",
    	"client_secret": "oVnLGLO8qUXYiPcEbodS7UdkvYu05UGU"
    }
    ```
    

| field | type | Description |
| --- | --- | --- |
| client_id | string  | The client ID for the application |
| client_secret | string | The client secret for the application. |
| grant_type | string | The type of grant being requested. |
| refresh_token | string | The refresh token obtained during a previous authorization. |

### **Success Response:**

- Status: 200
- Content-Type: application/json
- access_token: The access token to be used for accessing protected resources.
- expires_in: The expiration time of the access token in seconds.
- refresh_expires_in: The expiration time of the refresh token in seconds.
- refresh_token: The new refresh token to be used for obtaining a new access token.
- token_type: The type of token returned.
- not-before-policy: The time before which the token cannot be used.
- session_state: The session state.
- scope: The scope of the access token.
- **Example of Content:**
    
    ```json
    {
        "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJmclh5ZmRaV0xrNEdnRTdYREhLb193bGliUEJkMFJNcWE3RUhMbVpoc0NJIn0.eyJleHAiOjE3MDg2MTk1NjksImlhdCI6MTcwODYxOTI2OSwianRpIjoiOWRlOWZkMzctYzdlZC00YzUxLWI0NjQtZWVjNGU5ZDFkNDdiIiwiaXNzIjoiaHR0cDovLzAuMC4wLjA6ODA4MC9yZWFsbXMvdm94LXNlcnZpY2UiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMTY4NjZmNTctZGM5Yi...",
        "expires_in": 300,
        "refresh_expires_in": 1800,
        "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI2MThmMjE2MC1jNjFkLTQyYjEtODM5NS0zNTM3MzZkZTVhMzYifQ.eyJleHAiOjE3MDg2MjEwNjksImlhdCI6MTcwODYxOTI2OSwianRpIjoiYmE1NDRjM2MtOGYzY...",
        "token_type": "Bearer",
        "not-before-policy": 0,
        "session_state": "42428296-d45f-4f12-81a1-0dfe63c135e3",
        "scope": "email profile"
    }
    ```
    

## Errors

This API uses the following error codes:

- `400 Bad Request`: The request was malformed or missing required parameters.
- `401 Unauthorized`: The API key provided was invalid or missing.
- `404 Not Found`: The requested resource was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
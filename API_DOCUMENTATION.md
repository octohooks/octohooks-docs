# Octohooks

Welcome to the Octohooks API documentation!

## Table of Contents

1. [Introduction](/docs/INTRODUCTION.md)
   - [Main Concepts](#main-concepts)
   - [Authentication](#authentication)
   - [HTTPBearer](#httpbearer)
   - [Code Samples](#code-samples)
   - [Idempotency](#idempotency)
   - [Cross-Origin Resource Sharing](#cross-origin-resource-sharing)
2. [Applications](#applications)
   - [List Applications](#list-applications)
   - [Create Application](#create-application)
   - [Get Application](#get-application)
   - [Update Application](#update-application)
   - [Delete Application](#delete-application)
3. [Endpoints](#endpoints)
   - [List Endpoints](#list-endpoints)
   - [Create Endpoint](#create-endpoint)
   - [Get Endpoint](#get-endpoint)
   - [Update Endpoint](#update-endpoint)
   - [Delete Endpoint](#delete-endpoint)
4. [Messages](#messages)
   - [Create Message](#create-message)

### Main Concepts

In Octohooks you have three important entities you will be interacting with:

- `messages`: these are the webhooks being sent. They can have contents and a few other properties.
- `application`: this is where messages are sent to. Usually you want to create one application for each user on your platform.
- `endpoint`: endpoints are the URLs messages will be sent to. Each application can have multiple `endpoints` and each message sent to that application will be sent to all of them (unless they are not subscribed to the sent event type).

### Authentication

Get your authentication token (`AUTH_TOKEN`) from the Octohooks dashboard and use it as part of the `Authorization` header as such: `Authorization: Bearer ${AUTH_TOKEN}`.

### HTTPBearer

**Security Scheme Type**: HTTP

**Authorization Scheme**: `bearer`

### Code Samples

The code samples assume you already have the respective libraries installed and you know how to use them. For the latest information on how to do that, please refer to the documentation.

### Idempotency

Octohooks supports idempotency for safely retrying requests without accidentally performing the same operation twice. This is useful when an API call is disrupted in transit and you do not receive a response.

To perform an idempotent request, pass the idempotency key in the `Idempotency-Key` header to the request. The idempotency key should be a unique value generated by the client. You can create the key in however way you like, though we suggest using UUID v4, or any other string with enough entropy to avoid collisions.

Octohooks' idempotency works by saving the resulting status code and body of the first request made for any given idempotency key for any successful request. Subsequent requests with the same key return the same result.

Please note that idempotency is only supported for `POST` requests.

### Cross-Origin Resource Sharing

This API features Cross-Origin Resource Sharing (CORS) implemented in compliance with [W3C spec](https://www.w3.org/TR/cors/). And that allows cross-domain communication from the browser. All responses have a wildcard same-origin which makes them completely public and accessible to everyone, including any code on any site.

## Applications

Applications are where messages are sent to. In most cases you would want to have one application for each of your users.

### List Applications

List of all the organization's applications.

```
GET https://api.octohooks.com/api/v1/applications
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Query Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| page           | **Optional**. The page number (default: 1)|
| page           | **Optional**. The page size (default: 10) |

#### Response

```json
[
  {
    "name": "My App",
    "uid": "my-app",
    "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
    "created": 1668595667207,
    "updated": 1668595667207
  }
]
```

### Create Application

Create a new application.

```
POST https://api.octohooks.com/api/v1/applications
```

#### Headers

| Name            | Description                        |
| --------------- | ---------------------------------- |
| Authorization   | **Required**. Bearer ${AUTH_TOKEN} |
| Idempotency-Key | **Optional**.                      |

#### Request

```json
{
  "name": "My App",
  "uid": "my-app"
}
```

#### Response

```json
{
  "name": "My App",
  "uid": "my-app",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Get Application

Get an application.

```
GET https://api.octohooks.com/api/v1/applications/{application_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |

#### Response

```json
{
  "name": "My App",
  "uid": "my-app",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Update Application

Update an application.

```
PUT https://api.octohooks.com/api/v1/applications/{application_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |

#### Request

```json
{
  "name": "My App",
  "uid": "my-app"
}
```

#### Response

```json
{
  "name": "My App",
  "uid": "my-app",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Delete Application

Delete an application.

```
DELETE https://api.octohooks.com/api/v1/applications/{application_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |

#### Response

```json
{
  "name": "My App",
  "uid": "my-app",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

## Endpoints

Endpoints are the URLs messages will be sent to. Each application can have multiple endpoints and each message sent to that application will be sent to all of them (unless they are not subscribed to the sent event type).

### List Endpoints

List the application's endpoints.

```
GET https://api.octohooks.com/api/v1/applications/{application_id}/endpoints
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |

#### Query Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| page           | **Optional**. The page number (default: 1)|
| page           | **Optional**. The page size (default: 10) |

#### Response

```json
{
  "data": [
    {
      "channels": [],
      "enabled": true,
      "eventTypes": ["user.created"],
      "name": "My Endpoint",
      "secrets": [],
      "uid": "my-endpoint",
      "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d",
      "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
      "created": 1668595667207,
      "updated": 1668595667207
    }
  ],
  "meta": {
    "count": 100
  }
}
```

### Create Endpoint

Create a new endpoint for the application.

```
POST https://api.octohooks.com/api/v1/applications/{application_id}/endpoints
```

#### Headers

| Name            | Description                        |
| --------------- | ---------------------------------- |
| Authorization   | **Required**. Bearer ${AUTH_TOKEN} |
| Idempotency-Key | **Optional**.                      |

#### Request

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "headers": {},
  "name": "My Endpoint",
  "uid": "my-endpoint",
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d"
}
```

#### Response

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "headers": {},
  "name": "My Endpoint",
  "secrets": [],
  "uid": "my-endpoint",
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Get Endpoint

Get an endpoint.

```
GET https://api.octohooks.com/api/v1/applications/{application_id}/endpoints/{endpoint_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |
| endpoint_id    | **Required**. The endpoint's ID or UID    |

#### Response

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "headers": {},
  "name": "My Endpoint",
  "secrets": [],
  "uid": "my-endpoint",
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Update Endpoint

Update an endpoint.

```
PUT https://api.octohooks.com/api/v1/applications/{application_id}/endpoints/{endpoint_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |
| endpoint_id    | **Required**. The endpoint's ID or UID    |

#### Request

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "headers": {},
  "name": "My Endpoint",
  "secrets": [],
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d"
}
```

#### Response

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "name": "My Endpoint",
  "secrets": [],
  "uid": "my-endpoint",
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

### Delete Endpoint

Delete an endpoint.

```
DELETE https://api.octohooks.com/api/v1/applications/{application_id}/endpoints/{endpoint_id}
```

#### Headers

| Name          | Description                        |
| ------------- | ---------------------------------- |
| Authorization | **Required**. Bearer ${AUTH_TOKEN} |

#### Path Parameters

| Name           | Description                               |
| -------------- | ----------------------------------------- |
| application_id | **Required**. The application's ID or UID |
| endpoint_id    | **Required**. The endpoint's ID or UID    |

#### Response

```json
{
  "channels": [],
  "enabled": true,
  "eventTypes": ["user.created"],
  "headers": {},
  "name": "My Endpoint",
  "secrets": [],
  "uid": "my-endpoint",
  "url": "https://webhook.site/130cbb57-09c5-492f-bbc3-2d532bddca9d",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

## Messages

### Create Message

Creates a new message and dispatches it to all of the application's endpoints.

The eventType indicates the type and schema of the event. All messages of a certain eventType are expected to have the same schema. Endpoints can choose to only listen to specific event types. Messages can also have channels, which similar to event types let endpoints filter by them. Unlike event types, messages can have multiple channels, and channels don't imply a specific message content or schema.

The payload property is the webhook's body (the actual webhook message). Octohooks supports payload sizes of up to ~350kb, though it's generally a good idea to keep webhook payloads small, probably no larger than 40kb.

```
POST https://api.octohooks.com/api/v1/applications/{application_id}/messages
```

#### Headers

| Name            | Description                        |
| --------------- | ---------------------------------- |
| Authorization   | **Required**. Bearer ${AUTH_TOKEN} |
| Idempotency-Key | **Optional**.                      |

#### Request

```json
{
  "channels": [],
  "eventType": "user.created",
  "uid": "my-message",
  "payload": {
    "created_at": "2022-11-16T10:47:47.207Z",
    "email": "john.smith@example.com",
    "email_verified": true,
    "family_name": "Smith",
    "given_name": "John",
    "locale": "en-GB",
    "name": "John Smith",
    "nickname": "johnsmith",
    "picture": "https://randomuser.me/api/portraits/men/81.jpg",
    "updated_at": "2022-11-16T10:47:47.207Z",
    "user_id": "google-oauth2|102857191578298947818"
  }
}
```

#### Response

```json
{
  "channels": [],
  "eventType": "user.created",
  "payload": {
    "created_at": "2022-11-16T10:47:47.207Z",
    "email": "john.smith@example.com",
    "email_verified": true,
    "family_name": "Smith",
    "given_name": "John",
    "locale": "en-GB",
    "name": "John Smith",
    "nickname": "johnsmith",
    "picture": "https://randomuser.me/api/portraits/men/81.jpg",
    "updated_at": "2022-11-16T10:47:47.207Z",
    "user_id": "google-oauth2|102857191578298947818"
  },
  "uid": "my-message",
  "id": "ada4692e-96a4-45c9-8a58-2d389f95454a",
  "created": 1668595667207,
  "updated": 1668595667207
}
```

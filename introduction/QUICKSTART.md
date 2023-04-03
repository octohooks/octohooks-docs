# Quickstart

This page has everything you need to start sending webhooks with Octohooks.

## Main Concepts

In Octohooks you have three important entities you will be interacting with:

- `messages`: these are the webhooks being sent. They can have contents and a few other properties.
- `application`: this is where messages are sent to. Usually you want to create one application for each user on your platform.
- `endpoint`: endpoints are the URLs messages will be sent to. Each application can have multiple `endpoints` and each message sent to that application will be sent to all of them (unless they are not subscribed to the sent event type).

For more information, please refer to the [Overview section](OVERVIEW.md).

## Getting started

Get your authentication token (`AuthToken`) from the [Octohooks dashboard](https://example.com).

## Sending messages

### Creating a consumer application

Each of your users needs an associated consumer application. The easiest way is to create a new application whenever a user signs up. In this section we will use the create application API endpoint to create an application.

You would need the application's ID when sending messages. You can either save the `ID` returned when creating the application, or set your own unique id (e.g. your user's username or internal database ID) in the optional `UID` field and use that instead.

Code example:

```typescript
import { Octohooks } from "octohooks";

const octohooks = new Octohooks("AUTH_TOKEN");

const application = await octohooks.application.create({
  name: "Application name",
  uid: "application-name",
});
```

### Send a message

We will now send a new message using the [create message API endpoint](../API_DOCUMENTATION.md#create-message). It accepts an `APP ID`, which is the application's ID (or custom `UID`) from the previous section. In addition, it accepts the following fields (as part the json body):

- `eventType`: an identifier denoting the type of the event. E.g. invoice.paid.
- `uid`: an optional unique `ID` for the event. This is useful if you want to map each message to unique events on your system.
  `payload`: a JSON dictionary that can hold anything. Its content will be sent as the webhook content.

For example, the following code sends a webhook of type `eventType`, with the contents of `payload` as the body:

```typescript
import { Octohooks } from "octohooks";

const octohooks = new Octohooks("AUTH_TOKEN");

await octohooks.message.create("app_teexfqn3o09y1fnmm631r0fo8uq", {
  channels: [],
  eventType: "customer.created",
  uid: "774fa91a-510b-4a35-9799-6c233f6af790",
  payload: {
    email: "customer@email.com",
    integration: 100032,
    domain: "test",
    customer_code: "cus_xnxdt6s1zg1f4nx",
    id: 1173,
    identified: false,
    identifications: null,
    createdAt: "2016-03-29T20:03:09.584Z",
    updatedAt: "2016-03-29T20:03:09.584Z",
  },
});
```

#### Idempotency

Octohooks supports idempotency for safely retrying requests without accidentally performing the same operation twice. This is useful when an API call is disrupted in transit and you do not receive a response.

For more information, please refer to the [idempotency section](../advanced/IDEMPOTENCY.md) of the docs.

Note: while the `UID` can potentially be used to enforce short-term uniqueness (similar to idempotency), it's recommended to use the idempotency mechanism when needed rather than using on the `UID` checks.

### Add webhook endpoints

In the example above we showed how to send messages, though these messages were not sent to any specific URLs. In order for them to be sent, we need to add endpoints. This is what this section is about.

You can use our API to add endpoints to your applications.

For example:

```typescript
import { Octohooks } from "octohooks";

const octohooks = new Octohooks("AUTH_TOKEN");

await octohooks.endpoint.create("app_teexfqn3o09y1fnmm631r0fo8uq", {
  channels: [],
  enabled: true,
  eventTypes: ["customer.created"],
  headers: {},
  name: "My main endpoint",
  uid: 'my-main-endpoint',
  url: "https://api.example.com/octohooks-webhooks",
});
```

## Using Octohooks in a stateless manner

You can use Octohooks in a completely stateless manner, without having to store any Octohooks identifiers (or anything else) in your own database; you can do it by utilizing `UID`s. If you set a `UID` on an application, endpoint, or any other entity, you can use this `UID` interchangeably with its `ID` anywhere in the API.

For more information, please refer to the section about `UID`s in the overview.

<p align="right"><a href="/introduction/OVERVIEW.md">Go to Overview</a></p>

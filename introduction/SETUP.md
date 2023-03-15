# Setup

Octohooks offers a REST API to interact with the service, and easy to use libraries to interact with the API.

Octohooks also has an API reference and OpenAPI (formerly Swagger) specifications at the API documentation page which you can always refer to the exact schemas and endpoints available in our API.

When working with Octohooks you can either use our REST API directly, or using any of our libraries. In this document we'll cover how to install them for each platform.

## JavaScript/TypeScript

Install the libraries

```bash
npm install octohooks
// Or
yarn add octohooks
```

Then you can just use them as follows

```typescript
import { Octohooks } from "octohooks";

const octohooks = new Octohooks("AUTH_TOKEN");

const message = await octohooks.message.create(application.id, {
    channels: [],
    eventType: 'user.created',
    payload: {
        email: 'foo.bar@example.com',
    },
});
```

## C#

Install the libraries

```bash
dotnet add package Octohooks.net
# or
NuGet\Install-Package Octohooks.net
```

```csharp
using Octohooks.net;
using Octohooks.net.Domain.Entities;

var octohooksClient = new OctohooksClient("AUTH_TOKEN");

var message = await octohooksClient.Message.Create("my-application", new Message()
{
    Channels = new string[0],
    EventType = "user.created",
    Payload = new 
    {
        Email = "foo.bar@example.com"
    }
});
```

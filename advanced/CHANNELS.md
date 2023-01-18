# Channels

Channels are an extra dimension of filtering messages that is orthogonal to event types. You can listen to multiple channels from each endpoint, and you can send each message to multiple channels.

Event types imply a specific consistent schema, and mean a specific type of message. Channels are filters based on the expected recipient or group of recipients.

Note: like event types, channels are meant to filter messages to a particular application, and not across applications.

## When not to use it

The channels are not meant as a way to filter messages across different customers, that's what applications are for. Applications provide proper isolation between messages of different customers, while channels do not.

Additionally, there are limits to the number of channels messages and endpoints can be associated with. And having applications with a large number of endpoints is very inefficient and can hurt performance.

## Example use-cases

Channels are useful for when you have a variety of sub-categories or recipients that expect the same types of messages but just need additional filtering.

For example, consider Github. You may want to define webhooks for the whole organization, but only send certain events to certain endpoints based on the repository. You could just create a Octohooks App per repository and then manually add the endpoints to each, but it makes for a much better experience to have the webhook handling defined in one place with the same endpoints listening to multiple projects.

So for example, you can have octohooks, octohooks/octohooks-clients and octohooks/octohooks-docs as channels, and then have Github send messages for both octohooks and for each repo whenever an event occurs on a specific repository. Github's customers can then create endpoints that listen to events either for the whole group, or for each repository in particular.

## Channels filtering rules

Channels are case-sensitive, and endpoints that are filtering for specific channels will only get messages sent to that specific channel.

Octohooks will send (or not send) to endpoints based on the following conditions:

* Endpoint has no channels set: this is a catch-all, all messages are sent to to it, regardless of whether the message had channels set.
* Both endpoint and message have channels set: if there's a shared channel between them, the message will be sent to the endpoint.
* Endpoint has channels set and message has no channels set: the message will not be sent to the endpoint.
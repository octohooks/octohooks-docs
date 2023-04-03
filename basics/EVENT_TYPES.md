# Event Types

Each message sent through Octohooks has an associated event type. Event types are identifiers denoting the type of message being sent and are the primary way for webhook consumers to configure what events they are interested in receiving.

Event types are just a string, for example: `user.signup`, `invoice.paid` and `workflow.completed`.

Webhook consumers can choose which events are sent to which endpoint. By default, all messages are sent to all endpoints. Though when adding or editing endpoints, users can choose to only subscribe to some of the event types for this particular endpoint.

## Event type format

Event types have a pattern defined a `^[a-zA-Z0-9\\-_.]+$`, meaning it can contain any of the following characters:

* A through Z (uppercase or lowercase)
* 0 through 9
* Special characters: `-`, `_`, `.`

<p align="right"><a href="/basics/API_KEYS.md">Go to API Keys</a></p>

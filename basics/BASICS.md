# Event Types

Each message sent through Octohooks has an associated event type. Event types are identifiers denoting the type of message being sent and are the primary way for webhook consumers to configure what events they are interested in receiving.

Event types are just a string, for example: `user.signup`, `invoice.paid` and `workflow.completed`.

Webhook consumers can choose which events are sent to which endpoint. By default, all messages are sent to all endpoints. Though when adding or editing endpoints, users can choose to only subscribe to some of the event types for this particular endpoint.

## Event type format

Event types have a pattern defined a `^[a-zA-Z0-9\\-_.]+$`, meaning it can contain any of the following characters:

* A through Z (uppercase or lowercase)
* 0 through 9
* Special characters: `-`, `_`, `.`

# API Keys

Manage your API keys to authenticate requests with Octohooks. Octohooks authenticates your API requests using your account’s API keys. If you don’t include your key when making an API request, or use an incorrect or outdated one, Octohooks returns an error.

**API keys are per environment**. Every organization starts with a development environment with a corresponding API key. This key should only be used for development or internal testing and is not intended to be used in any production systems.

## Obtaining your API keys

Your API keys can be found on the "API Access" page of the Octohooks Dashboard.

--IMAGE

## Keeping your keys safe

Your secret API key can be used to make any API call on behalf of your account. Treat your secret API key as you would any other password. Grant access only to those who need it. Ensure it is kept out of any version control system you may be using. Control access to your key using a password manager or secrets management service.

## Rotating keys

If an API key is compromised, rotate the key in the Dashboard to block it and generate a new one.

When you rotate an API key, the old key will be blocked immediately, so any systems currently using that key will experience downtime. This behavior will change in a future release.

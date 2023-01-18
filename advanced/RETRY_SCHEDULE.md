# Retry Schedule

Octohooks attempts to deliver each webhook message based on a retry schedule with exponential backoff.

## The schedule

Each message is attempted based on the following schedule, where each period is started following the failure of the preceding attempt:

* Immediately
* 5 seconds
* 5 minutes
* 30 minutes
* 2 hours
* 5 hours
* 10 hours
* 10 hours (in addition to the previous)

If an endpoint is removed or disabled delivery attempts to the endpoint will be disabled as well.

For example, an attempt that fails three times before eventually succeeding will be delivered roughly 35 minutes and 5 seconds following the first attempt.

## Failed delivery handling

After the conclusion of the above attempts the message will be marked as Failed for this endpoint, and you will get a webhook of type `message.attempt.exhausted` notifying you of this error.

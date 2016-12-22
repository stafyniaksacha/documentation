---
title: Sending metadata
---

# Sending metadata

In every request you send to Kuzzle, you can include a `metadata` object.
This object content will be ignored by Kuzzle,
but it will also be forwarded back in `responses` and in `notifications` (see below).

You can also include metadata information to a subscription request.
These metadata information will be forwarded to other subscribers at the moment of the subscription,
and when you leave the room. Please note that when leaving the room,
the forwarded metadata are those provided in the **subscription** request.

This feature is especially useful to include volatile information about the performed request.


## Updating a document

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "index": "<data index>",
  "collection": "<data collection>",
  "controller": "document",
  "action": "update",
  "_id": "<a document ID>",
  "body": {
    "somefield": "now has a new value"
  },
  "metadata": {
    "modifiedBy": "awesome me",
    "reason": "it needed to be modified"
  }
}
```

<section class="others"></section>

>The following `update` notification will be sent to all subscribers:

<section class="others"></section>

```litcoffee
{
  "status": 200,
  "error": null,
  "index": "<data index>",
  "collection": "<data collection>",
  "controller": "document",
  "action": "update",
  "state": "pending",
  "scope": "<in|out>",
  "metadata": {
    "modifiedBy": "awesome me",
    "reason": "it needed to be modified"
  },
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "a document ID",
    "_source": {
      "somefield": "now has a new value",
      "someOtherField": "was left unchanged"
    },
  }
}
```


## Subscribe to a room

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "index": "<data index>",
  "collection": "<data collection>",
  "controller": "realtime",
  "action": "subscribe",
  "body": {
    // subscription filters
  },
  "metadata": {
    "hello": "my name is Bob"
  }
}
```

<section class="others"></section>

>And then, if you leave the room, the other subscribers will receive the following notification:

<section class="others"></section>

```litcoffee
{
  "status": 200,
  "error": null,
  "index": "<data index>",
  "collection": "<data collection>",
  "controller": "realtime",
  "action": "unsubscribe",
  "state": "done",
  "scope": "out",
  "metadata": {
    "hello": "my name is Bob"
  },
  "requestId": "<unique request identifier>",
  "result": {
    "roomId": "<uniqueKuzzleRoomID>",
    "count": <the new user count on that room>
  }
}
```

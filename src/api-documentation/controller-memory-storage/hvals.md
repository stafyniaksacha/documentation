---
layout: side-code.html
algolia: true
language-tab:
  js: HTTP
  json: Other protocols
title: hvals
---

# hvals



<blockquote class="js">
<p>
**URL:** `http://kuzzle:7512/ms/_hvals/<key>`  
**Method:** `GET`
</p>
</blockquote>

<blockquote class="json">
<p>
**Query**
</p>
</blockquote>


```json
{
  "controller": "ms",
  "action": "hvals",
  "_id": "<key>"
}
```

>**Response**

```javascript
{
  "requestId": "<unique request identifier>",
  "status": 200,
  "error": null,
  "controller": "ms",
  "action": "hvals",
  "collection": null,
  "index": null,
  "volatile": null,
  "result": [
    "<value of field1>",
    "<value of field2>",
    "..."
  ]
}
```

Returns all values contained in a hash.

[[_Redis documentation_]](https://redis.io/commands/hvals)

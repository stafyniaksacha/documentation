---
layout: side-code.html
algolia: true
language-tab:
  js: HTTP
  json: Other protocols
title: srem
---

# srem




<blockquote class="js">
<p>
**URL:** `http://kuzzle:7512/ms/_srem/<key>`  
**Method:** `DELETE`  
**Body:**
</p>
</blockquote>


```js
{
  "members": ["member1", "member2", "..."]
}
```



<blockquote class="json">
<p>
**Query**
</p>
</blockquote>


```json
{
  "controller": "ms",
  "action": "srem",
  "_id": "<key>",
  "body": {
    "members": ["member1", "member2", "..."]
  }
}
```

>**Response**

```javascript
{
  "requestId": "<unique request identifier>",
  "status": 200,
  "error": null,
  "controller": "ms",
  "action": "srem",
  "collection": null,
  "index": null,
  "volatile": null,
  "result": "<number of removed members>"
}
```

Removes members from a set of unique values.

[[_Redis documentation_]](https://redis.io/commands/srem)

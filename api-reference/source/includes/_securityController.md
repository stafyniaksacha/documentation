# ~ security controller


## createOrReplaceProfile

<section class="http"></section>

>**URL:** `http://kuzzle:7511/profiles/<profile_id>`  
>**Method:** `PUT`  
>**Body**

<section class="http"></section>

```litcoffee
{
  // The new array of role IDs and restrictions (cannot be empty)
  "roles": [
    {
      "_id": "<roleID>"
    },
    {
      "_id": "<roleID>",
      "restrictedTo": [
        {
          "index": "<index>"
        },
        {
          "index": "<index>",
          "collections": [
            "<coll1>",
            "<coll2>"
          ]
        }
      ]
    },
    ...
  ]
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createOrReplaceProfile",

  "_id": "<the profile id>",
  // the profile definition
  "body": {
    // The new array of role IDs and restrictions (cannot be empty)
    "roles": [
      {
        "_id": "<roleID>"
      },
      {
        "_id": "<roleID>",
        "restrictedTo": [
          {
            "index": "<index>"
          },
          {
            "index": "<index>",
            "collections": [
              "<coll1>",
              "<coll2>"
            ]
          }
        ]
      },
      ...
    ]
  }
}
```

> Response

```litcoffee
{
  "status":200,                       // Assuming everything went well
  "error": null,                      // Assuming everything wen well
  "result": {
    "_id": "<profile_id>",
    "_index": "%kuzzle",
    "_type": "profiles",
    "_version": 1,
    "created": false,
  },
  "requestId": "<unique request identifier>",
  "controller": "security",
  "action": "createOrReplaceProfile",
  "metadata": {}
}
```

Creates (if no `_id` provided) or updates (if `_id` matches an existing one) a profile with a new list of roles.


## createOrReplaceRole

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/<role id>` or `http://kuzzle:7511/roles/<role id>/_createOrReplace`  
>**Method:** `PUT`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "indexes": {
    "_canCreate": true,
    "*": {
      "_canDelete": true,
      "collections": {
        "_canCreate": true,
        "*": {
          "_canDelete": true,
          "controllers": {
            "*": {
              "actions": {
                "*": true
              }
            }
          }
        }
      }
    }
  }
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createOrReplaceRole",

  "_id": "<the role id>",
  // the role definition
  "body": {
    "indexes": {
      "_canCreate": true,
      "*": {
        "_canDelete": true,
        "collections": {
          "_canCreate": true,
          "*": {
            "_canDelete": true,
            "controllers": {
              "*": {
                "actions": {
                  "*": true
                }
              }
            }
          }
        }
      }
    }
  }
}
```

>Response

```litcoffee
{
  "status":200,                       // Assuming everything went well
  error":null,                      // Assuming everything wen well
  "result": {
    "_id": "<role id>",
    "_index": "%kuzzle",
    "_type": "roles",
    "_version": 1,
    "created": true,
    "_source": {} // your role definition
  }
  "requestId": "<unique request identifier>",
  "controller": "security",
  "action": "createOrReplaceRole",
  "metadata": {},
}
```

Validates and stores a role in Kuzzle's persistent data storage.

The body content needs to match Kuzzle's role definition.
To get some more detailed information on the expected role definition,
please refer to [Kuzzle's role reference definition documentation](https://github.com/kuzzleio/kuzzle/blob/master/docs/security/roles-reference.md).

To get some more detailed information about Kuzzle's user management model,
please refer to [Kuzzle's security documentation](https://github.com/kuzzleio/kuzzle/blob/master/docs/security/).


## createOrReplaceUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>`  
>**Method:** `PUT`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "profileId": "<profile id>",            // mandatory. The profile id for the user
  "name": "John Doe",                     // Additional optional User properties
  ...
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createOrReplaceUser",

  "_id": "<the user id>",
  "body": {
    "profileId": "<profile id>",          // mandatory. The profile id for the user
    "name": "John Doe",                   // Additional optional User properties
    ...
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "users",
  "controller": "security",
  "action": "createOrReplaceUser",
  "metadata": {},
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id, either provided or auto-generated>",
    "_index": "%kuzzle",
    "_source": {
      "profileId": "<profile id>",
      "name": "John Doe",
      ...
    },
    "_type": "users",
    "_version": 1,
    "created": true
  }
}
```

Persist a `user` object to Kuzzle's database layer.

The `user` is created if it does not exists yet or replaced with the given object if it does.


## createProfile

<section class="http"></section>

>**URL:** `http://kuzzle:7511/profiles/<profile id>/_create`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  // The new array of role IDs and restrictions (cannot be empty)
  "policies": [
    {
      "_id": "<roleID>"
    },
    {
      "_id": "<roleID>",
      "restrictedTo": [
        {
          "index": "<index>"
        },
        {
          "index": "<index>",
          "collections": [
            "<coll1>",
            "<coll2>"
          ]
        }
      ]
    },
    ...
  ]
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createProfile",
  "_id": "<the profile id>",

  // the profile definition
  "body": {
    // The new array of role IDs and restrictions (cannot be empty)
    "policies": [
      {
        "_id": "<roleID>"
      },
      {
        "_id": "<roleID>",
        "restrictedTo": [
          {
            "index": "<index>"
          },
          {
            "index": "<index>",
            "collections": [
              "<coll1>",
              "<coll2>"
            ]
          }
        ]
      },
      ...
    ]
  }
}
```

> Response

```litcoffee
{
  "status":200,                       // Assuming everything went well
  "error": null,                      // Assuming everything wen well
  "result": {
    "_id": "<profile_id>",
    "_index": "%kuzzle",
    "_type": "profiles",
    "_version": 1,
    "created": true,
    "_source": {} // your profile definition
  },
  "requestId": "<unique request identifier>",
  "controller": "security",
  "action": "createProfile",
  "metadata": {},
}
```

Creates a profile with a new list of roles.

**Note:** The `_id` parameter is mandatory.


## createRole

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/<role id>/_create`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "indexes": {
    "_canCreate": true,
    "*": {
      "_canDelete": true,
      "collections": {
        "_canCreate": true,
        "*": {
          "_canDelete": true,
          "controllers": {
            "*": {
              "actions": {
                "*": true
              }
            }
          }
        }
      }
    }
  }
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createRole",
  "_id": "<the role id>",

  // the role definition
  "body": {
    "indexes": {
      "_canCreate": true,
      "*": {
        "_canDelete": true,
        "collections": {
          "_canCreate": true,
          "*": {
            "_canDelete": true,
            "controllers": {
              "*": {
                "actions": {
                  "*": true
                }
              }
            }
          }
        }
      }
    }
  }
}
```

>Response

```litcoffee
{
  "status":200,                       // Assuming everything went well
  error":null,                      // Assuming everything wen well
  "result": {
    "_id": "<role id>",
    "_index": "%kuzzle",
    "_type": "roles",
    "_version": 1,
    "created": true,
    "_source": {} // your role definition
  }
  "requestId": "<unique request identifier>",
  "controller": "security",
  "action": "createRole",
  "metadata": {},
}
```

Validates and stores a role in Kuzzle's persistent data storage.
**Note:** The `_id` parameter is mandatory.

The body content needs to match Kuzzle's role definition.
To get some more detailed information on the expected role definition,
please refer to [Kuzzle's role reference definition documentation](https://github.com/kuzzleio/kuzzle/blob/master/docs/security/roles-reference.md).

To get some more detailed information about Kuzzle's user management model,
please refer to [Kuzzle's security documentation](https://github.com/kuzzleio/kuzzle/blob/master/docs/security/).


## createUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>/_create` or `http://kuzzle:7511/users/_create`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "profileIds": ["<profile id>"],         // Mandatory. The profile ids for the user
  "name": "John Doe",                     // Additional optional User properties
  ...
}

// example with a "local" authentication

{
  "profileIds": ["<profile id>"],         // Mandatory. The profile ids for the user
  "name": "John Doe",                     // Additional optional User properties
  ...
  "password": "MyPassword"                // ie: Mandatory for "local" authentication plugin
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createUser",
  "_id": "<the user id>",                 // Optional. If not provided, will be generated automatically.

  "body": {
    "profileIds": ["<profile id>"],       // Mandatory. The profile ids for the user
    "name": "John Doe",                   // Additional optional User properties
    ...
  }
}

// example with a "local" authentication

{
  "controller": "security",
  "action": "createUser",
  "_id": "<the user id>",                 // Optional. If not provided, will be generated automatically.

  "body": {
    "profileIds": ["<profile id>"],       // Mandatory. The profile ids for the user
    "name": "John Doe",                   // Additional optional User properties
    ...
    "password": "MyPassword"              // ie: Mandatory for "local" authentication plugin
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "users",
  "controller": "security",
  "action": "createUser",
  "metadata": {},
  "scope": null,
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id, either provided or auto-generated>",
    "_index": "%kuzzle",
    "_source": {
      "profileIds": ["<profile id>"],
      "name": "John Doe",
      ...
    },
    "_type": "users",
    "_version": 1,
    "created": true
  }
}
```

Creates a new `user` in Kuzzle's database layer.

If an `_id` is provided in the query and if a `user` already exists with the given `_id`, an error is returned.
If not provided, the `_id` will be auto-generated.

Provided profile ids are used to set the permissions of the user.

Other mandatory additional informations are needed depending on the authentication plugins installed you want to use.


## createRestrictedUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>/_createRestricted` or `http://kuzzle:7511/users/_createRestricted`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "name": "John Doe",                     // Additional optional User properties
  ...
}

// example with a "local" authentication

{
  "name": "John Doe",                     // Additional optional User properties
  ...
  "password": "MyPassword"                // ie: Mandatory for "local" authentication plugin
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "createRestrictedUser",

  "_id": "<the user id>",                 // Optional. If not provided, will be generated automatically.
  "body": {
    "name": "John Doe",                   // Additional optional User properties
    ...
  }
}

// example with a "local" authentication

{
  "controller": "security",
  "action": "createRestrictedUser",

  "_id": "<the user id>",                 // Optional. If not provided, will be generated automatically.
  "body": {
    "name": "John Doe",                   // Additional optional User properties
    ...
    "password": "MyPassword"              // ie: Mandatory for "local" authentication plugin
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "users",
  "controller": "security",
  "action": "createRestrictedUser",
  "metadata": {},
  "scope": null,
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id, either provided or auto-generated>",
    "_index": "%kuzzle",
    "_source": {
      "profileIds": ["<profile id>"],
      "name": "John Doe",
      ...
    },
    "_type": "users",
    "_version": 1,
    "created": true
  }
}
```

Creates a new `user` in Kuzzle's database layer.

If an `_id` is provided in the query and if a `user` already exists with the given `_id`, an error is returned.
If not provided, the `_id` will be auto-generated.

Profile ids are set accordingly to the Kuzzle configuration.
This route is especially useful to allow anonymous users to create a user.

Other mandatory additional informations are needed depending on the authentication plugins installed you want to use.


## deleteProfile

<section class="http"></section>

>**URL:** `http://kuzzle:7511/_profiles/<profile_id>`  
>**Method:** `DELETE`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "deleteProfile",

  // The profile unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a profile.
  "_id": "<profile ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    "_id": "<Profile ID>",        // The profile ID
  },
  "index": "%kuzzle",
  "collection": "profiles",
  "action": "deleteProfile",
  "controller": "security",
  "requestId": "<unique request identifier>"
}
```

Given a `profile id`, deletes the corresponding profile from the database. Note
that the related roles will NOT be deleted.


## deleteRole

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/<role id>`  
>**Method:** `DELETE`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "deleteRole",

  // The role unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a role.
  "_id": "<role ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    "_id": "<Unique role ID>"         // The role ID
  }
  "index": "%kuzzle",
  "collection": "roles"
  "action": "deleteRole",
  "controller": "security",
  "requestId": "<unique request identifier>"
}
```

Given a `role id`, delete the corresponding role from the database.


## deleteUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>`  
>**Method:** `DELETE`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "deleteUser",

  // The role unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a role.
  "_id": "<role ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    "_id": "<Unique role ID>"         // The role ID
  }
  "index": "%kuzzle",
  "collection": "users",
  "action": "deleteUser",
  "controller": "security",
  "requestId": "<unique request identifier>"
}
```

Given a `user id`, deletes the corresponding `user` from Kuzzle's database layer.


## getProfile

<section class="http"></section>

>**URL:** `http://kuzzle:7511/_profiles/<profile_id>`  
>**Method:** `GET`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "getProfile",

  // The profile unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a profile.
  "_id": "<profile ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    "_id": "<Unique profile ID>",    // The profile ID
    "_source": {                      // The requested profile
      ...
    },
    "index": "%kuzzle",
    "collection": "profiles"
    "action": "getProfile",
    "controller": "security",
    "requestId": "<unique request identifier>"
  }
}
```
Given a `profile id`, retrieves the corresponding profile from the database.


## getProfileRights

<section class="http"></section>

>**URL:** `http://kuzzle:7511/_profiles/<profile_id>/_rights`  
>**Method:** `GET`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "getProfileRights",

  // The profile unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a profile.
  "_id": "<profile ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    // An array of objects containing the profile rights
    "hits": [
      {
        "controller": "<ctrl_name|*>",
        "action": "<action_name|*>",
        "index": "<index_name|*>",
        "collection": "<collection_name|*>",
        "value": "<allowed|denied|conditional>"
      },
      {
        // Another rights item... and so on
      }
    ],
}
```
Given a `profile id`, retrieves the corresponding rights.


## getRole

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/<role id>`  
>**Method:** `GET`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "getRole",

  // The role unique identifier. It's the same one that Kuzzle sends you
  // in its responses when you create a role.
  "_id": "<role ID>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    "_id": "<Unique role ID>",        // The role ID
    "_source": {
      "indexes": {
        ...
      }
    }
  },
  "index": "%kuzzle",
  "collection": "roles"
  "action": "getRole",
  "controller": "security",
  "metadata": {},
  "requestId": "<unique request identifier>"
}
```

Given a `role id`, retrieves the corresponding role from the database.


## getUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>`  
>**Method:** `GET`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "getUser",
  "_id": "<user id>"
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "<data index>",
  "collection": "<data collection>",
  "controller": "security",
  "action": "getUser",
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id>",
    "_source": {
      "profileId": "<profile id>",
      ...                         // The user object content
    }
  }
}
```


Given a `user id`, gets the matching user from Kuzzle's dabatase layer.


## getUserRights

<section class="http"></section>

>**URL:** `http://kuzzle:7511/_users/<user_id>/_rights`  
>**Method:** `GET`

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "getUserRights",
  "_id": "<user ID>"
}
```


>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result": {
    // An array of objects containing the user rights
    "hits": [
      {
        "controller": "<ctrl_name|*>",
        "action": "<action_name|*>",
        "index": "<index_name|*>",
        "collection": "<collection_name|*>",
        "value": "<allowed|denied|conditional>"
      },
      {
        // Another rights item... and so on
      }
    ],
}
```
Given a `user id`, gets the matching user's rights from Kuzzle's dabatase layer.


## mGetProfiles

<section class="http"></section>

>**URL:** `http://kuzzle:7511/profiles/_mget`  
>**Method:** `POST`  
>**Body:**

<section class="http"></section>

```litcoffee
{
  "body": {
    // ids must be an array of profile ids
    "ids": ["myFirstProfile", "MySecondProfile"]
  }
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "mGetProfiles",
  "body": {
    // ids must be an array of profile ids
    "ids": ["myFirstProfile", "MySecondProfile"]
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "profiles"
  "action": "mGetProfiles",
  "controller": "security",
  "requestId": "<unique request identifier>",
  "result": {
    "_source": {
      ...
    }
  }
}
```

Retrieves a list of `profile` objects from Kuzzle's database layer given a list of profile ids.


## mGetRoles

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/_mget`  
>**Method:** `POST`  
>**Body:**

<section class="http"></section>

```litcoffee
{
  "body": {
    // ids must be an array of role id
    "ids": ["myFirstRole", "MySecondRole"]
  }
}
```

<section class="others"></section>

>Query

<section class="websocket"></section>

```litcoffee
{
  "controller": "security",
  "action": "mGetRoles",
  "body": {
    // ids must be an array of role id
    "ids": ["myFirstRole", "MySecondRole"]
  }
}
```

>Response

```litcoffee
{
  "action": "mGetRoles",
  "collection": "roles",
  "controller": "security",
  "error": null,
  "index": "%kuzzle",
  "metadata": {},
  "requestId": "<unique request identifier>",
  "result":
  {
     "_shards": {
       "failed": 0,
       "successful": 5,
       "total": 5
     },
     "hits": [
       {
         "_id": "test",
         "_index": "%kuzzle",
         "_score": 1,
         "_source": {
           "_id": "test",
           "indexes": {} // Rights for each indexes, controllers, ... can be found here
         },
         "_type": "roles"
       }
     ],
     "max_score": null,
     "timed_out": false,
     "took": 1,
     "total": 1
  },
  "status": 200
}
```

Retrieves a list of `role` objects from Kuzzle's database layer given a list of role ids.


## searchProfiles

<section class="http"></section>

>**URL:** `http://kuzzle:7511/profiles/_search[?from=0][&size=42]`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  // A roles array containing a list of role IDs can be added
  "policies":  [
    "myrole",
    "admin"
  ],
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "searchProfiles",
  "body": {
    // A roles array containing a list of role IDs can be added
    "policies":  [
      "myrole",
      "admin"
    ]
  },
  // filter can handle pagination using the from and size properties
  "from": 0,
  "size": 42
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "result":
  {
    "_shards": {
      "failed": 0,
      "successful": 5,
      "total": 5
    },
    "hits": [
      {
        "_id": "my-profile-1",
        "_source": {
          "policies": [
            "_id": "myroleId"
          ]
        }
      },
      {
        "_id": "my-profile-2",
        "_source": {
          "policies": [
            "_id": "myroleId"
          ]
        }
      }
    ],
    "max_score": 1,
    "timed_out": false,
    "took": 1,
    "total": 2
    },
    "index": "%kuzzle",
    "collection": "profiles"
    "action": "searchProfiles",
    "controller": "security",
    "requestId": "<unique request identifier>"
  }
}
```

Retrieves profiles referring to a given set of roles in their policies.

Attribute `policies` in body is optional.

The `from` and `size` arguments allow pagination.

Available filters:

| Filter | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``policies`` | array | Contains an array `policies` with a list of role ids | ``undefined`` |


## searchRoles

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/_search[?from=0][&size=42]`  
>**Method:** `POST`  
>**Body:**

<section class="http"></section>

```litcoffee
{
  // indexes must be an array of index
  "indexes": ["myindex"]
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "searchRoles",
  "body": {
    "indexes": ["myindex"],
    // "from" and "size" argument for pagination
    "from": 0,
    "size": 10
  }
}
```

>Response

```litcoffee
{
  "action": "searchRoles",
  "collection": "roles",
  "controller": "security",
  "error": null,
  "index": ""%kuzzle",
  "metadata": {},
  "requestId": "<unique request identifier>",
  "result":
  {
     "_shards": {
       "failed": 0,
       "successful": 5,
       "total": 5
     },
     "hits": [
       {
         "_id": "test",
         "_index": "%kuzzle",
         "_score": 1,
         "_source": {
           "_id": "test",
           "indexes": {} // Rights for each indexes, controllers, ... can be found here
         },
         "_type": "roles"
       }
     ],
     "max_score": null,
     "timed_out": false,
     "took": 1,
     "total": 1
  },
  "status": 200
}
```

Retrieves all roles with rights defined for given `indexes`.

Attribute `indexes` in body is optional.

The `from` and `size` arguments allow pagination.

Available filters:

| Filter | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``indexes`` | array | List of indexes id related to the searched role | ``undefined`` |


## searchUsers

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/_search[?from=0][&size=42]`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
  "filter": {
    "and": [
      {
        "in": {
          "profileId": ["anonymous", "default"],
        }
      },
      {
        "geo_distance": {
          "distance": "10km",
          "pos": {
            "lat": "48.8566140",
            "lon": "2.352222"
          }
        }
      }
    ]
  }
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "searchUsers",
  "body": {
    "filter": { // If empty or missing, Kuzzle will return all users.
      "and": [
        {
          "in": {
            "profileId": [
              "anonymous",
              "default"
            ],
          }
        },
        {
          "geo_distance": {
            "distance": "10km",
            "pos": {
              "lat": "48.8566140",
              "lon": "2.352222"
            }
          }
        }
      ]
    }
  },
  // "from" and "size" argument for pagination
  "from": 0,
  "size": 10
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "users",
  "action": "search",
  "controller": "security",
  "requestId": "<unique request identifier>",
  "result": {
    "total": <total number of users matching the filter>,
    // An array of user objects
    "hits": [
      {
        "_id": "<user id>",
        "_source": { ... }             // The user object content
      },
      {
        ...
      }
    ]
  }
}
```

Retrieves all the users matching the given filter.

The `from` and `size` arguments allow pagination.


## updateProfile

<section class="http"></section>

>**URL:** `http://kuzzle:7511/profiles/<profile id>`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
    "policies": [
      {
        "_id": "<roleID>"
      },
      {
        "_id": "<roleID>",
        "restrictedTo": [
          {
            "index": "<index>"
          },
          {
            "index": "<index>",
            "collections": [
              "<coll1>",
              "<coll2>"
            ]
          }
        ]
      },
      ...
    ]
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "updateProfile",
  "_id": "<profile id>",
  "body": {
    "policies": [
      {
        "_id": "<roleID>"
      },
      {
        "_id": "<roleID>",
        "restrictedTo": [
          {
            "index": "<index>"
          },
          {
            "index": "<index>",
            "collections": [
              "<coll1>",
              "<coll2>"
            ]
          }
        ]
      },
      ...
    ]
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "profiles",
  "action": "updateProfile",
  "controller": "security",
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id>",
    "_index": "%kuzzle",
    "_type": "profiles",
    "_version": 2
  }
}
```

Given a `profile id`, updates the matching Profile object in Kuzzle's database layer.


## updateRole

<section class="http"></section>

>**URL:** `http://kuzzle:7511/roles/<role id>`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
    "indexes": {
      "some index": {
        "collections": {
          "some collection": {
            "_canDelete": true
          }
        }
      }
    }
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "updateRole",
  "_id": "<role id>",
  "body": {
    "indexes": {
      "some index": {
        "collections": {
          "some collection": {
            "_canDelete": true
          }
        }
      }
    }
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "roles",
  "action": "updateRole",
  "controller": "security",
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id>",
    "_index": "%kuzzle",
    "_type": "roles",
    "_version": 2
  }
}
```

Given a `role id`, updates the matching Role object in Kuzzle's database layer.

The body content needs to match Kuzzle's role definition.

<aside class="warning">
Unlike a regular document update, this method will replace the whole role definition by the body content.<br>
In other words, you always need to provide the complete role definition in the body.
</aside>
To get some more detailed information on the expected role definition, please refer to
[Kuzzle's role reference definition documentation](https://github.com/kuzzleio/kuzzle/blob/beta/docs/security/roles-reference.md).

To get some more detailed information about Kuzzle's user management model,
please refer to [Kuzzle's security documentation](https://github.com/kuzzleio/kuzzle/blob/master/docs/security/).


## updateUser

<section class="http"></section>

>**URL:** `http://kuzzle:7511/users/<user id>`  
>**Method:** `POST`  
>**Body**

<section class="http"></section>

```litcoffee
{
    "foo": "bar",                    // Some properties to update
    "name": "Walter Smith",
    ...
}
```

<section class="others"></section>

>Query

<section class="others"></section>

```litcoffee
{
  "controller": "security",
  "action": "updateUser",
  "_id": "<user id>",
  "body": {
    "foo": "bar",                    // Some properties to update
    "name": "Walter Smith",
    ...
  }
}
```

>Response

```litcoffee
{
  "status": 200,                      // Assuming everything went well
  "error": null,                      // Assuming everything went well
  "index": "%kuzzle",
  "collection": "users",
  "action": "updateUser",
  "controller": "security",
  "requestId": "<unique request identifier>",
  "result": {
    "_id": "<user id>",
    "_index": "%kuzzle",
    "_type": "users",
    "_version": 2
  }
}
```

Given a `user id`, updates the matching User object in Kuzzle's database layer.
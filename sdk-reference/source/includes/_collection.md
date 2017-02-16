# Collection

In Kuzzle, you manipulate documents and subscriptions, both related to data collections.

A data collection is a set of data managed by Kuzzle. It acts like a data table for persistent documents, or like a room for pub/sub messages.


## Constructors


```js
var dataCollection = new Collection(kuzzle, "my-collection", "my-index")
```

```java
  Collection myCollection = new Collection(kuzzle, "my-collection", "my-index");
```


```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Collection;

$kuzzle = new Kuzzle('localhost');
$dataCollection = new Collection($kuzzle, 'my-collection', 'my-index');
```

### Collection(kuzzle, collection, index)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``kuzzle`` | object | Kuzzle object |
| ``collection`` | string | The name of the data collection you want to manipulate |
| ``index`` | string | Name of the index containing the data collection |

**Note:** It's recommended to instantiate a Collection object by calling [Kuzzle.collection](#collection) rather than using the constructor directly

## Properties

| Property name | Type | Description | get/set |
|--------------|--------|-----------------------------------|---------|
| ``kuzzle`` | object | linked kuzzle instance | get |
| ``index`` | object | Name of the index containing the data collection | get |
| ``collection`` | string | The name of the data collection handled by this instance | get |
| ``headers`` | object | Headers for all sent documents. | get/set |


**Note:** the ``headers`` property is inherited from the main ``Kuzzle`` object and can be overrided

## collectionMapping

```js
var dataMapping = kuzzle
  .collection('collection', 'index')
  .collectionMapping({someField: {type: 'string', index: 'analyzed'}})
  .apply();
```

```java
CollectionMapping dataMapping = kuzzle
  .collection("collection", "index")
  .collectionMapping(new JSONObject().put("someFiled", new JSONObject().put("type", "string").put("index", "analyzed"))
  .apply();
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\DataMapping;

$mapping = [
  'someField' => [
    'type' => 'string',
    'index' => 'analyzed'
  ]
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $dataMapping = $dataCollection->collectionMapping($mapping);

  // $dataMapping instanceof DataMapping
  $dataMapping->apply();
}
catch (ErrorException $e) {

}
```

Creates a new `CollectionMapping` object, using its constructor.

### collectionMapping([mapping])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``mapping`` | JSON Object | Optional mapping |

### Return value

Returns the newly created `CollectionMapping` object.

## count

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .count({}, function (err, res) {
    // ...
  });

// Using promises (NodeJS only)
kuzzle
 .collection('collection', 'index')
 .countPromise({})
 .then(res => {
   // ...
 });
```

```java
JSONObject filters = new JSONObject();

kuzzle
  .collection("collection", "index")
  .count(filters, new ResponseListener<Integer>() {
    @Override
    public void onSuccess(Integer object) {
      // Handle success
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;

$filters = [];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $count = $dataCollection->count($filters);
}
catch (ErrorException $e) {
}
```

> Callback response:

```json
12
```

<aside class="notice">There is a small delay between documents creation and their existence in our search layer, usually a couple of seconds. That means that a document that was just been created won't be returned by this function</aside>

Returns the number of documents matching the provided set of filters.

### count(filters, [options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``filters`` | JSON Object | Filters in [ElasticSearch Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/query-dsl.html) format |
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Callback handling the response |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |


### Callback response

Resolves to the matched documents count as a ``integer``.

## create

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .create(function (error, result) {
    // callback called once the create operation has completed
    // => the result is a JSON object containing the raw Kuzzle response
  });

// Using promises (NodeJS only)
kuzzle
 .collection('collection', 'index')
 .createPromise()
 .then(result => {
   // promise resolved once the create operation has completed
   // => the result is a JSON object containing the raw Kuzzle response
 });
```

```java
kuzzle
  .collection("collection", "index")
  .create(new ResponseListener<JSONObject>() {
    @Override
    public void onSuccess(JSONObject object) {
      // callback called once the create operation has completed
      // => the result is a JSON object containing the raw Kuzzle response
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $result = $dataCollection->create();
}
catch (ErrorException $e) {

}
```

> Callback response:

```json
{
  "status": 200,
  "error": null,
  "requestId": "cf1fc8b4-fd87-46c3-b0a2-3d9d2fb7d401",
  "controller": "write",
  "action": "createCollection",
  "collection": "newly created collection",
  "index": "index",
  "metadata": {},
  "state": "done",
  "scope": null,
  "result": {}
}
```

Create a new empty data collection, with no associated mapping.

### create([options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to a `JSON object` containing the raw Kuzzle response.

## createDocument

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .createDocument('foobar', {title: 'foo', content: 'bar'}, {updateIfExist: true}, function (err, res) {
    // callback called once the create action has been completed
    // => the result is a Document object
  });

// Using promises (NodeJS only)
kuzzle
 .collection('collection', 'index')
 .createDocumentPromise('foobar', {title: 'foo', content: 'bar'}, {updateIfExist: true})
 .then(res => {
   // promise resolved once the create action has been completed
   // => the result is a Document object
 });
```

```java
Document myDocument = new Document(collection);
myDocument.setContent("title", "foo");
myDocument.setContent("content", "bar");

kuzzle
  .collection("collection", "index")
  .createDocument(myDocument, new ResponseListener<Document>() {
    @Override
    public void onSuccess(Document object) {
      // callback called once the create action has been completed
      // => the result is a Document object
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;

$documentId = 'foobar';
$documentContent = [
  'title' => 'foo',
  'content' => 'bar'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $document = $dataCollection->createDocument($documentContent, $documentId);

  // $document instanceof Document
}
catch (ErrorException $e) {

}
```

Create a new document in Kuzzle.

### createDocument(Document, [options], [callback])

### createDocument([id], content, [options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``Document`` | object | Document object |
| ``id`` | string | Optional document identifier |
| ``content`` | JSON object | Content of the document to create |
| ``options`` | JSON object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``metadata`` | JSON object | Additional information passed to notifications to other users | ``null`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |
| ``refresh`` | string | If set to ``wait_for``, Kuzzle will wait the peristence layer indexation to return (available with Elasticsearch 5.x and above) | ``undefined`` |
| ``updateIfExist`` | boolean | If the same document already exists: throw an error if sets to ``false``. Update the existing document otherwise | ``false`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to a `Document` object containing the newly created document.

## delete

<aside class="warning">
Since the 2.0 version, ElasticSearch <a href="https://www.elastic.co/guide/en/elasticsearch/reference/2.4/breaking_20_mapping_changes.html#_types_may_no_longer_be_deleted">removed the ability to delete a Type</a>, which is used internally by Kuzzle to store its collections.<br>
<br>
As such, it is not possible to delete a collection from Kuzzle either. You can still delete all the documents that belong to a collection but if you need to delete the collection mapping, you will need to use a new index.
</aside>

## deleteDocument

```js
// Deleting one document using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .deleteDocument('document unique ID', function (err, res) {
    // callback called once the delete action has been completed
  });

// Deleting one document using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .deleteDocumentPromise('document unique ID')
  .then(res => {
    // promises resolved once the delete action has been completed
  });

// Deleting multiple documents using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .deleteDocument({filter: {equals: {title: 'foo'}}}, function (err, res) {
    // callback called once the delete with query has been completed
  });

// Deleting multiple documents using promises (NodeJS)
 kuzzle
 .collection('collection', 'index')
 .deleteDocumentPromise({filter: {equals: {title: 'foo'}}})
 .then(res => {
   // promise resolved once the delete by query has been completed
 });
```

```java
// Deleting one document
kuzzle
  .collection("collection", "index")
  .deleteDocument("document unique ID", new ResponseListener<String>() {
    @Override
    public void onSuccess(String object) {
      // The resulting string contains the deleted document ID
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });

// Deleting multiple documents
JSONObject equalsFilter = new JSONObject()
  .put("filter", new JSONObject()
  .put("equals",
    new JSONObject().put("title", "foo")
  ));

kuzzle
  .collection("collection", "index")
  .deleteDocument(equalsFilter, new ResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] object) {
      // The resulting object contains the list of deleted document IDs
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

// Deleting one document
try {
  $result = $dataCollection->deleteDocument('documentId');
}
catch (ErrorException $e) {

}

// Deleting multiple documents
$filters = [
  'filter' => [
    'equals' => ['field' => 'value']
  ]
];

try {
  $result = $dataCollection->deleteDocument($filters);
}
catch (ErrorException $e) {

}
```

> Callback response:

```json
[ "AVCoeBkimsySTKTfa8AX" ]
```

<aside class="notice">
There is a small delay between documents creation and their existence in our search layer, usually a couple of seconds. That means that a document that was just been created won't be deleted by the filtered version of this function
</aside>

Delete either a stored document, or all stored documents matching search filters.

### deleteDocument(documentId, [options], [callback])

### deleteDocument(filters, [options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``documentId`` | string | Unique document identifier |
| ``filters`` | JSON object | Filters in [ElasticSearch Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/query-dsl.html) format |
| ``options`` | JSON object | Optional parameters |
| ``callback`` | function | Optional callback |


Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``metadata`` | JSON object | Additional information passed to notifications to other users | ``null`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |
| ``refresh`` | string | If set to ``wait_for``, Kuzzle will wait the peristence layer indexation to return (available with Elasticsearch 5.x and above) | ``undefined`` |


### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to an `array` containing the deleted document IDs.

## document

```js
var document = kuzzle
  .collection('collection', 'index')
  .document('id', {some: 'content'})
  .save();
```

```java
Document document = kuzzle
  .collection("collection", "index")
  .document("id", new JSONObject().put("some", "content"))
  .save();
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;

$documentId = 'foobar';
$documentContent = [
  'title' => 'foo',
  'content' => 'bar'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');
$document = $dataCollection->document($documentId, $documentContent);

// $document instanceof Document

try {
  $document->save();
}
catch (ErrorException $e) {

}
```

Creates a new `Document` object, using its constructor.

### document([id], [content])

| Arguments | Type | Description |
|-----------|------|-------------|
| ``id`` | string | Optional document unique ID |
| ``content`` | JSON object | Optional document content |

### Return value

Returns the newly created `Document` object.

## fetchDocument

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .fetchDocument('documentId', function (error, result) {
    // result is a Document object
  });

// Using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .fetchDocumentPromise('documentId')
  .then(result => {
    // result is a Document object
  });
```

```java
kuzzle
  .collection("collection", "index")
  .fetchDocument("documentId", new ResponseListener<Document>() {
    @Override
    public void onSuccess(Document object) {
      // result is a Document object
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;

$documentId = 'foobar';

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $document = $dataCollection->fetchDocument($documentId);

  // $document instanceof Document
}
catch (ErrorException $e) {

}
```

Retrieves a single stored document using its unique document ID.

### fetchDocument(documentId, [options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``documentId`` | string | Unique document identifier |
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Callback handling the response |


Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |


### Callback response

Resolves to a `Document` object.

## fetchAllDocuments

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .fetchAllDocuments(function (error, result) {
    // result is an object containing the total number of documents
    // and an array of Document objects
  });

// Using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .fetchAllDocumentsPromise()
  .then(result => {
    // result is an object containing the total number of documents
    // and an array of Document objects
  });
```

```java
kuzzle
  .collection("collection", "index")
  .fetchAllDocuments(new ResponseListener<DocumentList>() {
    @Override
    public void onSuccess(DocumentList object) {
      // result is an object containing the total number of documents
      // and an array of Document objects
      for (Document doc : result.getDocuments()) {
        // Get documents
      }

      result.getTotal(); // return total of documents returned
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;
use \Kuzzle\Util\SearchResult;

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $result = $dataCollection->fetchAllDocuments();

  // $result instanceof SearchResult

  foreach($result->getDocuments() as $document) {
    // $result instanceof Document
  }
}
catch (ErrorException $e) {

}
```

> Callback response:

```json
{ "total": 3,
  "documents": [<Document>, <Document>, <Document>]
}
```

Retrieves all documents stored in this data collection.

### fetchAllDocuments([options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Callback handling the response |


Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``from`` | integer | Upper bounds for paginated results. | ``0`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |
| ``size`` | integer | Lower bounds for paginated results. | ``10`` |

### Callback response

Resolves to an object containing:

- the total number of retrieved documents
- a `array` of `Document` objects


## getMapping

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .getMapping(function (error, result) {
    // result is a CollectionMapping object
  });

// Using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .getMappingPromise()
  .then(result => {
    // result is a CollectionMapping object
  });
```

```java
kuzzle
  .collection("collection", "index")
  .getMapping(new ResponseListener<CollectionMapping>() {
    @Override
    public void onSuccess(CollectionMapping object) {
      // result is a CollectionMapping object
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\DataMapping;

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $mapping = $dataCollection->getMapping();

  // $mapping instanceof DataMapping
}
catch (ErrorException $e) {

}
```

Retrieves the current mapping of this collection.

### getMapping([options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``options`` | JSON object | Optional parameters |
| ``callback`` | function | Callback handling the response |


Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |

### Callback response

Resolves to a `CollectionMapping` object.

## publishMessage

```js
kuzzle
  .collection('collection', 'index')
  .publishMessage({foo: 'bar', baz: 'qux'});
```

```java
JSONObject message = new JSONObject().put("some", "content");
JSONObject metadata = new JSONObject().put("metadata", "are volatile information");
Options opts = new Options().setMetadata(metadata);

kuzzle
  .collection("collection", "index")
  .publish(message, opts);
```

```php
<?php

use \Kuzzle\Kuzzle;

$message = [
  'field' => 'value'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $result = $dataCollection->publishMessage($message);
}
catch (ErrorException $e) {

}
```

Publish a realtime message

### publishMessage(Document, [options], [callback])

### publishMessage(content, [options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``Document`` | object | Document object |
| ``content`` | JSON Object | Content of the document to publish |
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``metadata`` | JSON Object | Additional information passed to notifications to other users | ``null`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to a raw Kuzzle response in JSON format.

## replaceDocument

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .replaceDocument('documentId', {title: 'foo', content: 'bar'}, function (error, result) {
    // result is a Document object
  });

// Using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .replaceDocumentPromise('documentId', {title: 'foo', content: 'bar'})
  .then(result => {
    // result is a Document object
  });
```

```java
JSONObject newContent = new JSONObject("new", "document content");

kuzzle
  .collection("collection", "index")
  .replaceDocument("documentId", newContent, new ResponseListener<Document>() {
    @Override
    public void onSuccess(Document document) {

    }

    @Override
    public void onError(JSONObject error) {

    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;

$documentId = 'foobar';
$documentContent = [
  'field' => 'value'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $document = $dataCollection->replaceDocument($documentId, $documentContent);

  // $document instanceof Document
}
catch (ErrorException $e) {

}
```

Replace an existing document with a new one.

### replaceDocument(documentId, content, [options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``documentId`` | string | Unique document identifier |
| ``content`` | JSON Object | Content of the document to create |
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``metadata`` | JSON Object | Additional information passed to notifications to other users | ``null`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |
| ``refresh`` | string | If set to ``wait_for``, Kuzzle will wait the peristence layer indexation to return (available with Elasticsearch 5.x and above) | ``undefined`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to an updated `Document` object.

## room

```js
var room = kuzzle
  .collection('collection', 'index')
  .room()
  .renew({in: {field: ['some', 'new', 'filter']}}, function (err, res) {
    // handle notifications
  });
```

```java
JSONObject filters = new JSONObject()
  .put("in",
    new JSONObject("field")
      .put(new JSONArray()
        .put("some")
        .put("filter")
      )
  );

Room room = kuzzle.collection("collection", "index")
  .room()
  .renew(filters, new ResponseListener<NotificationResponse>() {
    @Override
      public void onSuccess(NotificationResponse object) {
        // handle notifications
      }

      @Override
      public void onError(JSONObject error) {
        // Handle notifications error
      }
  });
```

```php
<?php

// not implemented (this SDK uses HTTP and is thus stateless)
```

Creates a new `Room` object, using its constructor.

### room([options])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``options`` | object | Subscription configuration |

### Return value

Returns the newly created `Room` object.

## search

```js
var filter = {
  filter: {
    and: [
      {
        in: {
          status: ['idle', 'wantToHire', 'toHire', 'riding'],
        }
      },
      {
        in:{
          type: ['cab']
        }
      },
      {
        geo_distance: {
          distance: '10km',
          pos: {
            lat: '48.8566140', lon: '2.352222'
          }
        }
      }
    ]
  },
  aggregations: {
    aggs_name: {
      terms: {
        field: "field_name"
      }
    }
  }
};

kuzzle
  .collection('collection', 'index')
  .search(filter, function (err, res) {
    res.documents.forEach(document => {
      console.log(document.toString());
    });
  });

// Using promises (NodeJS only)
kuzzle
  .collection('collection', 'index')
  .searchPromise({})
  .then(res => {
    res.documents.forEach(document => {
      console.log(document.toString());
    });
  });
```

```java
JSONObject filter = new JSONObject()
  .put("filter", new JSONObject()
    .put("and", new JSONArray()
      .put(
        new JSONObject().put("in",
          new JSONObject().put("status",
            new JSONArray()
              .put("idle")
              .put("wantToHire")
              .put("toHire")
              .put("riding")
          )
        )
      )
      .put(
        new JSONObject().put("in",
          new JSONObject()
            .put("type", new JSONArray().put("cab"))
        )
      )
      .put(
        new JSONObject().put("geo_distance",
          new JSONObject()
            .put("distance", "10km")
            .put("pos",
              new JSONObject()
                .put("lat", "48.8566140")
                .put("lon", "2.352222")
            )
        )
      )
    )
  )
  .put("aggregations", new JSONObject()
    .put("aggs_name", new JSONObject()
      .put("terms", new JSONObject()
        .put("field", "field_name")
      )
    )
  );

kuzzle
  .collection("collection", "index")
  .search(userFilter, new ResponseListener<DocumentList>() {
    @Override
    public void onSuccess(DocumentList result) {
      for (Document doc : result.getDocuments()) {
        // Get documents
      }

      result.getTotal(); // return total of documents returned

      result.getAggregations(): // return a JSONObject representing the aggregations response
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;
use \Kuzzle\Util\SearchResult;

$filters = [
  'filter' => [
    'and' => [
      [
        'in' => [
          'status' => ['idle', 'wantToHire', 'toHire', 'riding'],
        ]
      ],
      [
        'in' => [
          'type' => ['cab']
        ]
      ],
      [
        'geo_distance' => [
          'distance' => '10km',
          'pos' => [
            'lat' => '48.8566140',
            'lon' => '2.352222'
          ]
        ]
      ]
    ]
  ],
  'aggregations' => [
    'aggs_name' => [
      'terms' => [
        'field' => 'field_name'
      ]
    ]
  ]
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $searchResult = $dataCollection->search($filters);

  // $searchResult instanceof SearchResult
  $searchResult->getTotal();

  foreach($searchResult->getDocuments() as $document) {
    // $document instanceof Document
  }

  // return an array representing the aggregations response
  $searchResult->getAggregations();
}
catch (ErrorException $e) {

}
```

> Callback response:

```json
{ "total": 3,
  "documents": [<Document>, <Document>, <Document>],
  "aggregations": {
    "aggs_name": {"aggregation": "object"}
  }
}
```


<aside class="notice">
There is a small delay between documents creation and their existence in our search layer, usually a couple of seconds. That means that a document that was just been created won't be returned by this function
</aside>

Executes a search on the data collection.

### search(filters, [options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``filters`` | JSON object | Filters in [ElasticSearch Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/query-dsl.html) format |
| ``options`` | JSON object | Optional parameters |
| ``callback`` | function | Callback handling the response |


Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |


### Callback response

Resolves to a `JSON object` containing:

- the total number of matched documents
- an `array` of `Document` objects
- an `array` of `aggregations` objects if some are provided in the request (see the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.0/search-aggregations.html) for more details)

## setHeaders

```js
kuzzle
  .collection('collection', 'index')
  .setHeaders({
    someContent: 'someValue',
    metadata: { someMetaData: ['with', 'some', 'values']}
  }, true);
```

```java
JSONObject headers = new JSONObject()
  .put("someContent", "someValue")
  .put("metadata", new JSONObject()
    .put("someMetaData", new JSONArray()
      .put("with")
      .put("some")
      .put("values")
    )
  );

kuzzle
  .collection("collection", "index")
  .setHeaders(content, true);
```

```php
<?php

use \Kuzzle\Kuzzle;

$headers = [
  'someContent' => 'someValue'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');
$dataCollection->setHeaders($headers, true);
```

This is a helper function returning itself, allowing to easily set headers while chaining calls.

### setHeaders(content, [replace])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``content`` | JSON Object | New content |
| ``replace`` | boolean | true: replace the current content with the provided data, false: merge it |

**Note:** by default, the ``replace`` argument is set to ``false``

### Return value

Returns the `Collection` object to allow chaining.

## subscribe

```js
kuzzle
  .collection('collection', 'index')
  .subscribe({equals: {title: 'foo'}}, function (error, result) {
    // called each time a new notification on this filter is received
    // check the Room/Notifications section of this documentation
    // to get notification examples
  })
  .onDone(function (err, roomObject) {
    // Handles the subscription result. Can be chained.
  });
```

```java
JSONObject filter = new JSONObject()
  .put("and", new JSONArray()
    .put(
      new JSONObject().put("in",
        new JSONObject().put("status",
          new JSONArray()
            .put("idle")
            .put("wantToHire")
            .put("toHire")
            .put("riding")
        )
      )
    )
    .put(
      new JSONObject().put("in",
        new JSONObject()
          .put("type", new JSONArray().put("cab"))
      )
    )
    .put(
      new JSONObject().put("geo_distance",
        new JSONObject()
          .put("distance", "10km")
          .put("pos",
            new JSONObject()
              .put("lat", "48.8566140")
              .put("lon", "2.352222")
          )
      )
    )
  );

kuzzle
  .collection("collection", "index")
  .subscribe(filter, new ResponseListener<NotificationResponse>() {
    @Override
    public void onSuccess(NotificationResponse object) {
      // called each time a new notification on this filter is received

      // check the Room/Notifications section of this documentation
      // to get notification examples
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  })
  .onDone(new ResponseListener<Room>() {
    @Override
    public void onSuccess(Room response) {
      // Handle subscription success
    }

    @Override
    public void onError(JSONObject error) {
      // Handle subscription error
    }
  });
```

```php
<?php

// not implemented (this SDK uses HTTP and is thus stateless)
```

Subscribes to this data collection with a set of filters.

<aside class="notice">
To subscribe to the entire data collection, simply provide an empty filter.
</aside>

### subscribe(filters, [options], callback)

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``filters`` | JSON Object | [Filters](/real-time-filters) |
| ``options`` | object | (Optional) Subscription configuration. Passed to the Room constructor. |
| ``callback`` | function | Callback to call every time a notification is received on this subscription |

### Return value

Returns an object exposing the following method:  
  ```onDone(callback)```

The `callback` argument is called when the subscription ends, either successfully or with an error.

## truncate

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .truncate(function (error, result) {
    // callback called once the truncate operation has completed
    // => the result is a JSON object containing the raw Kuzzle response
  });

// Using promises (NodeJS only)
kuzzle
 .collection('collection', 'index')
 .truncatePromise()
 .then(result => {
   // promise resolved once the truncate operation has completed
   // => the result is a JSON object containing the raw Kuzzle response
 });
```

```java
kuzzle
  .collection("collection", "index")
  .truncate(new ResponseListener<JSONObject>() {
    @Override
    public void onSuccess(JSONObject object) {
      // callback called once the truncate operation has completed
      // => the result is a JSON object containing the raw Kuzzle response
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $dataCollection->truncate();
}
catch (ErrorException $e) {

}
```

> Callback response:

```json
{
  "status": 200,
  "error": null,
  "requestId": "8fdc0efb-6fc7-427d-a3a1-fd8cf5eabc20",
  "controller": "admin",
  "action": "truncateCollection",
  "collection": "name of the truncated collection",
  "index": "name of the index containing the truncated collection",
  "metadata": {},
  "state": "done",
  "result": { "acknowledged": true }
}
```

Truncate the data collection, removing all stored documents but keeping all associated mappings.

This method is a lot faster than removing all documents using a query.

### truncate([options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``options`` | JSON Object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to a `JSON object` containing the raw Kuzzle response.

## updateDocument

```js
// Using callbacks (NodeJS or Web Browser)
kuzzle
  .collection('collection', 'index')
  .updateDocument('documentId', {title: 'foo', content: 'bar'}, function (error, result) {
    // result is a Document object
  });

// Using promises (NodeJS)
kuzzle
  .collection('collection', 'index')
  .updateDocumentPromise('documentId', {title: 'foo', content: 'bar'})
  .then(result => {
    // result is a Document object
  });
```

```java
JSONObject newTitle = new JSONObject().put("title", "a shiny new title");

kuzzle
  .collection("collection", "index")
  .updateDocument("documentId", newTitle, new ResponseListener<Document>() {
    @Override
    public void onSuccess(Document result) {
      // result is an updated Document object
    }

    @Override
    public void onError(JSONObject error) {
      // Handle error
    }
  });
```

```php
<?php

use \Kuzzle\Kuzzle;
use \Kuzzle\Document;

$documentId = 'foobar';
$documentContent = [
  'newField' => 'foo'
];

$kuzzle = new Kuzzle('localhost');
$dataCollection = $kuzzle->collection('collection', 'index');

try {
  $document = $dataCollection->updateDocument($documentId, $documentContent);

  // $document instanceof Document
}
catch (ErrorException $e) {

}
```

Update parts of a document, by replacing some fields or adding new ones.
Note that you cannot remove fields this way: missing fields will simply be left unchanged.

### updateDocument(documentId, content, [options], [callback])

| Arguments | Type | Description |
|---------------|---------|----------------------------------------|
| ``documentId`` | string | Unique document identifier |
| ``content`` | JSON object | Content of the document to create |
| ``options`` | JSON object | Optional parameters |
| ``callback`` | function | Optional callback |

Available options:

| Option | Type | Description | Default |
|---------------|---------|----------------------------------------|---------|
| ``metadata`` | JSON object | Additional information passed to notifications to other users | ``null`` |
| ``queuable`` | boolean | Mark this request as (not) queuable | ``true`` |
| ``refresh`` | string | If set to ``wait_for``, Kuzzle will wait the persistence layer indexation to return (available with Elasticsearch 5.x and above) | ``undefined`` |

### Return value

Returns the `Collection` object to allow chaining.

### Callback response

Resolves to an up-to-date `Document` object.
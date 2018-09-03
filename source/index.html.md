---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Items

`Item` objects stored information about buyers' requested items. Once created, these objects will be stored in both mongoDB and Elasticsearch. Shippers can search and view certain fields but only buyers have the permission to edit/delete items.

Field | Type | Description
----- | ---- | -----------
_id | ObjectId | Unique identifier for the object.
name | string | The item's name, meant to be displayable to shippers.
description | string | The item's description, meant to be displayable to shippers.
buyer | ObjectId | Unique identifier of the person who requested the item.
price | number | The item's price, meant to be displayable to shippers. This price does not include shipper fee and Primor's charge.
requiredDate | number | Latest date buyer wish to receive the item, meant to be displayable to shippers.
shipperCountry | string | Country in which the item is available for purchase, meant to be available to shippers.
status | number | Current status of the item.

## Create an item

Create a new item object.

### HTTP Request

`POST /protected/items`

### Parameters

Field | Type | Description
-------- | ---- | -----------
name | string | The item's name, meant to be displayable to shippers.
description | string | The item's description, meant to be displayable to shippers.
buyer | string | Unique identifier of the person who requested the item.
price | number | The item's price, meant to be displayable to shippers. This price does not include shipper fee and Primor's charge.
requiredDate | number | Latest date buyer wished to receive the item, meant to be displayable to shippers.
shipperCountry | string | Country in which the item is available for purchase, meant to be available to shippers.
status | number | Current status of the item.

### Return

Return the new `itemId` if create succeeded. Otherwise return return an `Error`.

### Errors

Error code | Description
---------- | -----------
400 | At least one of the required fields is missing.
500 | Either mongoDB or Elasticsearch fail to create item. 

## Edit an item

Edit an existing item object. The function should be called with at least one of the fields specified.

### HTTP Request
`PUT /protected/items/:id`

### Parameters

Field | Type | Description
-------- | ---- | -----------
id | string | Unique identifier for the item.
name | string | The item's name, meant to be displayable to shippers.
description | string | The item's string, meant to be displayable to shippers.
price | number | The item's price, meant to be displayable to shippers.
requiredDate | number | Latest date buyer wish to receive the item, meant to be displayable to shippers.
shipperCountry | string | Country in which the item is available for purchase, meant to be available to shippers.

### Return

Return a response with status code 200 if edit succeeded. Otherwise return an `Error`.

### Errors

Error code | Description
---------- | -----------
500 | Either mongoDB or Elasticsearch fail to edit item.

## Delete an item

Delete an existing item object.

### HTTP Request

`DELETE /protected/items/:id`

### Parameters

Field | Type | Description 
-------- | ---- | -----------
id | string | Unique identifier for the item.

### Return

Return a response with status code 200 if delete succeeded. Otherwise return an<br> `Error`.

### Errors

Error code | Description
---------- | -----------
500 | Either mongoDB or Elasticsearch fail to delete item.

## Search items

Find items that match certain criterias.

### HTTP Request

`POST /protected/search`

### Parameters

Field | Type | Description
-------- | ---- | -----------
minPrice | number | Minimum item price. Items must have `price` greater or equal `minPrice` in order to be returned as search result.
maxPrice | number | Maximum item price. Items must have `price` smaller or equal `maxPrice` in order to be returned as search result.
minDate | number | Earliest possible `requiredDate`. Items must have `requiredDate` greater or equal `minDate` in order to be returned as search result.
maxDate | number | Latest possible `requiredDate`. Items must have `requiredDate` smaller or equal `maxDate` in order to be returned as search result.
shipperCountry | string | Country in which the item is available for purchase. Items must have matching `shipperCountry` in order to be returned as search result.

### Return

Return an array of items matching search criterias if succeeded. If no items match the criterias, return empty array. If Elasticsearch fails, return an `Error`.

### Errors

Error code | Description
---------- | -----------
500 | Elasticsearch fails. 


# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete


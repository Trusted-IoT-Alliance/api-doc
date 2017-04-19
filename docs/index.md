# TIA API Server v1.0.0

Trusted IoT Alliance API

- [RegistrantGroup](#registrantgroup)
	- [Find Registrants](#find-registrants)
	- [Get a Registrant](#get-a-registrant)
	- [Create a Registrant](#create-a-registrant)
	
- [RegistryThingGroup](#registrythinggroup)
	- [Find Things](#find-things)
	- [Read a Thing](#read-a-thing)
	- [Create Things](#create-things)
	
- [SpecGroup](#specgroup)
	- [Find Specs](#find-specs)
	- [Read a Spec](#read-a-spec)
	- [Create Spec](#create-spec)
	


# RegistrantGroup

## Find Registrants

<p>Allows users to view all organizations they have access to</p>

	GET /api/1.0/registrant

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| blockchain_id			| String			| **optional** <p>Used to find Organizations that have Registrants on a specific blockchain</p>							|

### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/registrant \
-H 'Authorization: Bearer $JWT' \
```

### Success Response

Success-Response:

```
HTTP/1.1 200 OK
[
  {
    "blockchain_ids": ["ethereum-mainnet"]
    "blockchain_addresses": [
      "0xc257274276a4e539741ca11b590b9447b26a8051"
    ]
    "name": "Apple",
    "description": "A technology company",
    "destination_blockchains": [{
      "blockchain_id": "ethereum-mainnet",
      "address": "0x048875e897b5877e5cdbec601a0ec3069880fe9f",
      "is_multi_access": true,
      "co_owner_addresses": [
        "0xc257274276a4e539741ca11b590b9447b26a8051"
      ]
    }]
  }
]
```
## Get a Registrant

<p>Allows users to read an organizations they have access to</p>

	GET /api/1.0/registrant/:id

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| id			| String			|  <p>Blockchain address that is used to identify an organization</p>							|

### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/registrant/0x048875e897b5877e5cdbec601a0ec3069880fe9f \
-H 'Authorization: Bearer $JWT' \
```

### Success Response

Success-Response:

```
HTTP/1.1 200 OK
{
  "blockchain_ids": ["ethereum-mainnet"]
  "blockchain_addresses": [
    "0xc257274276a4e539741ca11b590b9447b26a8051"
  ]
  "name": "Apple",
  "description": "A technology company",
  "destination_blockchains": [{
    "blockchain_id": "ethereum-mainnet",
    "address": "0x048875e897b5877e5cdbec601a0ec3069880fe9f",
    "is_multi_access": true,
    "co_owner_addresses": [
      "0xc257274276a4e539741ca11b590b9447b26a8051"
    ]
  }]
}
```
### Error Response

Error-Response:

```
HTTP/1.1 400 Bad Request
{
  "error": "10004",
  "message": "Not authorized to perform this operation",
}
```
## Create a Registrant

<p>By supplying an organization_id, a superuser can create an Open Registry Registrant for that organization by supplying the necessary fields</p>

	POST /api/1.0/registrant

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| name			| String			|  <p>The name of the Registrant</p>							|
| description			| String			|  <p>A description of the Registrant</p>							|
| destination_blockchains			| Object[]			|  <p>an array of the blockchains the registrants are deployed to</p>							|
| destination_blockchains.blockchain_id			| String			|  <p>the id of the destination blockchain</p>							|
| destination_blockchains.address			| String			| **optional** <p>the address for the registrant for an already deployed acccount. If none is provided, one will automatically be created</p>							|
| destination_blockchains.is_multi_access			| Boolean			| **optional** <p>A boolean which indicates whether the address points to a multi-access contract or not</p>							|
| destination_blockchains.co_owner_addresses			| String[]			| **optional** <p>the addresses for co-owners of the account if the address points to a multi-access contract</p>							|

### Examples

Example Usage

```
curl --request POST \
--url http://discovery.chronicled.com/api/1.0/registrant \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer $JWT' \
--data '{
   "name": "Apple",
   "description": "A technology company",
   "destination_blockchains": [{
     "name": "ethereum-mainnet"
   }]
}'
```

### Success Response

Successful Response

```
{
  "__v":0,
  "updated_at":"2016-08-05T20:39:33.236Z",
  "__t":"RegistrantCreation",
  "_id":"avkMAvS",
  "created_at":"2016-08-05T20:39:33.234Z" ,
  "registrant": {
    "name": "Apple",
    "description": "A technology company",
    "destination_blockchains": [{
      "blockchain_id": "ethereum-mainnet"
      "address":"0x123",
      "is_multi_access": "false"
    }]
  }
}
```
### Error Response

MissingProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The property 'destination_blockchains' is missing"
 }
```
InvalidProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The value ${destination_blockchains} for destination_blockhains is not valid. Reason: The value supplied is not an array
 }
```
NotAuthorized

```
 HTTP/1.1 400 Bad Request
{
 "error":"10004",
 "message":"Provided parameter is invalid",
    "detail": "Not authorized. Entity 'Registrant', id 'n/a', permission 'create'."
 }
```
# RegistryThingGroup

## Find Things

<p>This endpoint is used to find all Thing records that match a specific query</p>

	GET /api/1.0/thing


### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/thing
```

### Success Response

Success-Response:

```
HTTP/1.1 200 OK
[{
 "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
 "urn_identities": ["urn:protocol:123"],
 "spec_name": "product",
 "thing": {
   "urn_identities": ["urn:protocol:123"],
   "properties": {
     "product": {
       "title": "Nike Shoes 1",
       "subtitle": "Cool Shoes"
     }
   }
 },
 "spec": {
   "name": "product",
   "json_schema": {
     "type": "object",
     "properties": {
       "title": {"type": "string"}
       "subtitle": {"type": "string"}
     }
   }
 }
 "blockchain_records": {
   "ethereum-mainnet": {
     "state": "pending",
     "state_description": "transaction: \"0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8\"",
     "transaction_hash": "0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8",
     "history": [
       {
         "state_description": "transaction: \"0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8\"",
         "state": "pending",
         "date": "2017-04-11T16:57:23.113Z",
         "transaction_hash": "0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8",
         "_id": "58ed0af30f49637f1eb120aa"
       }
     ],
     "explorer_thing_link": "http://explorer.chronicled.org/#/thing/urn:protocol:123",
     "explorer_transaction_link": "http://explorer.chronicled.org/#/transaction/0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8"
   }
 },
 "registrant": {
   "name": "Apple",
   "description": "A technology company",
   "destination_blockchains": [{
     "blockchain_id": "ethereum-mainnet"
     "address":"0x123",
     "is_multi_access": "false"
   }]
 },
}]
```
## Read a Thing

<p>This endpoint is used to request all the information available about a Thing by supplying one of the Thing's identifying URN's as the id parameter</p>

	GET /api/1.0/thing/:id


### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| id			| String			|  <p>the urn_identity that maps to one recorded Thing</p>							|

### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/thing/nfc:1.0:AFE204250
```

### Success Response

Success-Response:

```
HTTP/1.1 200 OK
{
 "thing_id": "ayi21Z",
 "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
 "urn_identities": ["urn:protocol:123"],
 "spec_name": "product",
 "thing": {
   "urn_identities": ["urn:protocol:123"],
   "properties": {
     "product": {
       "title": "Nike Shoes 1",
       "subtitle": "Cool Shoes"
     }
   }
 },
 "spec": {
   "name": "product",
   "json_schema": {
     "type": "object",
     "properties": {
       "title": {"type": "string"}
       "subtitle": {"type": "string"}
     }
   }
 }
 "blockchain_records": {
   "ethereum-mainnet": {
     "state": "pending",
     "state_description": "transaction: \"0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8\"",
     "transaction_hash": "0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8",
     "history": [
       {
         "state_description": "transaction: \"0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8\"",
         "state": "pending",
         "date": "2017-04-11T16:57:23.113Z",
         "transaction_hash": "0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8",
         "_id": "58ed0af30f49637f1eb120aa"
       }
     ],
     "explorer_thing_link": "http://explorer.chronicled.org/#/thing/urn:protocol:123",
     "explorer_transaction_link": "http://explorer.chronicled.org/#/transaction/0xb79b7aba96e60d53fa324a5863cc14d9ee2fcdc3fb10bfe0d0195936b764e5c8"
   }
 },
 "registrant": {
   "name": "Apple",
   "description": "A technology company",
   "destination_blockchains": [{
     "blockchain_id": "ethereum-mainnet"
     "address":"0x123",
     "is_multi_access": "false"
   }]
 },
}
```
### Error Response

Error-Response:

```
HTTP/1.1 400 Invalid Request
{
  "error": "10020",
  "message": "Instance not found. Entity 'Thing' with urn 'urn:value:invalid'."
}
```
## Create Things

<p>Allows an user with access to an Organization to create a Thing or multiple Things for that Organization. The <code>spec_name</code> and <code>spec_organization_id</code> combination fetches a Spec, whose <code>json_schema</code> field determines what values can be supplied in the <code>properties</code> field of the Thing and the <code>sync</code> parameter determines what fields in <code>properties</code> are synchronized with a Blockchain. See <a href="#api-SpecGroup">Create Spec</a> for more details about the Spec resource</p>

	POST /api/1.0/thing

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| multiple			| Boolean			|  <p>Flag that specifies whether multiple things should be created or just one</p>							|
| urn_identities			| String[]			|  <p>An Array of URNS which uniquely identify this thing. Must be unique across all Things</p>							|
| registrant_address			| String			|  <p>The address of the Registrant the Thing will be deployed under</p>							|
| spec_name			| String			|  <p>The field which references the Spec that this thing is registered under</p>							|
| properties			| Object			|  <p>An object that contains the property and value pairs for the Thing</p>							|

### Examples

Example Usage for Single Thing

```
curl --request POST \
--url http://discovery.chronicled.com/api/1.0/thing \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer $JWT' \
--data '{
   "urn_identities": ["urn:protocol:123"],
   "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
   "spec_name": "product",
   "properties": {
     "title": "Nike Shoes 1"
     "subtitle": "Cool Shoes"
   }
}'
```
Example Usage for Multiple Things

```
curl --request POST \
--url http://discovery.chronicled.com/api/1.0/thing \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer $JWT' \
--data '{
   "multiple": true,
   "urn_identities": [
     ["ble:1.0:abc", "pbk:ec:secp256r1:abc"],
     ["ble:1.0:def", "pbk:ec:secp256r1:def"]
     ["ble:1.0:efg", "pbk:ec:secp256r1:efg"]
   ],
   "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
   "spec_name": "product_chronicled",
   "properties": {
     "title": "Nike Shoes 1"
     "subtitle": "Cool Shoes"
   },
}'
```

### Success Response

Successful Response for Thing

```
{
  "__v":0,
  "updated_at":"2016-08-05T20:39:33.236Z",
  "__t":"ThingCreation",
  "_id":"avkMAvS",
  "created_at":"2016-08-05T20:39:33.234Z",
  "things": [{
    "urn_identities": ["urn:protocol:123"],
    "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
    "spec_name": "product_chronicled",
    "properties": {
      "title": "Nike Shoes 1"
      "subtitle": "Cool Shoes"
    }
  }],
  "failed_things": []
}
```
Successful Response for Multiple Things

```
{
  "__v":0,
  "updated_at":"2016-08-05T20:39:33.236Z",
  "__t":"ThingCreation",
  "_id":"avkMAvS",
  "created_at":"2016-08-05T20:39:33.234Z",
  "things": [{
    "urn_identities": ["ble:1.0:abc", "pbck:ec:secp256r1:abc"],
    "spec_name": "product",
    "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
    "properties": {
      "title": "Nike Shoes 1"
      "subtitle": "Cool Shoes"
    },
  }, {
    "urn_identities": ["ble:1.0:def", "pbk:ec:secp256r1:def"]
    "spec_name": "product_chronicled",
    "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
    "properties": {
        "title": "Nike Shoes 1"
        "subtitle": "Cool Shoes"
    },
  }],
  "failed_things": [{
    "urn_identities": ["ble:1.0:efg", "pbk:ec:secp256r1:efg"],
    "reason_for_failure": "The urns provided are in use by another existing thing"
  }]
}
```
### Error Response

MissingProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The property 'urn_identities' is missing"
 }
```
InvalidProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The value ${spec_name} for spec_name is not valid. Reason: The spec_name does not match a spec on record"
 }
```
NotAuthorized

```
 HTTP/1.1 400 Bad Request
{
 "error":"10004",
 "message":"Provided parameter is invalid",
 "detail": "Not authorized. Entity 'Thing', id 'n/a', permission 'create'."
 }
```
# SpecGroup

## Find Specs

<p>Allows a user to view all the Specs they have access to</p>

	GET /api/1.0/registrant/:registrant_address/spec

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| registrant_address			| String			|  <p>The address of the Registrant that the Specs are registered with</p>							|

### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/registrant/0xc257274276a4e539741ca11b590b9447b26a8051/spec \
-H 'Authorization: Bearer $JWT' \
```

### Success Response

Success-Response:

```
HTTP/1.1 200 OK
[
 {
   "name": "product",
   "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
   "json_schema": {
     "type": "object"
     "properties": {
       "title": {"type": "string"},
       "subtitle": {"type": "string"}
     },
   },
   "sync": {
     "ethereum-mainneet": {
       "protobuf_json": {
         "package": "Product",
         "messages": [{
           "name": "Product",
           "fields": [
             {"rule": "required", "type": "string", "name": "title", "id": 1},
             {"rule": "required", "type": "string", "name": "subtitle", "id": 2}
           ]
         }]
       }
     }
   },
   "registrant": {
     "name": "Apple",
     "description": "A technology company",
     "destination_blockchains": [{
       "blockchain_id": "ethereum-mainnet"
       "address":"0x123",
       "is_multi_access": "false"
     }]
   }
 }
]
```
### Error Response

Error-Response:

```
HTTP/1.1 404 Not Found
{
  "error": "10004",
  "message": "Not authorized to perform this operation",
  "detail": "Not Authorized. Entity 'Registry-SpecView', id 'n/a', permission 'find'."
}
```
## Read a Spec

<p>Allows a user to read a Spec they have access to</p>

	GET /api/1.0/registrant/:registrant_address/spec/:name

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| registrant_address			| String			|  <p>The address of the Registrant that the Spec is registered with</p>							|
| name			| String			|  <p>The name of the spec</p>							|

### Examples

Example Usage

```
curl --request GET \
--url http://discovery.chronicled.com/api/1.0/registrant/0xc257274276a4e539741ca11b590b9447b26a8051/spec/product \
-H 'Authorization: Bearer $JWT' \
```

### Success Response

Successful response 

```
{
  "name": "product",
  "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
  "json_schema":  {
    "meta_schema" : "https://github.com/epoberezkin/ajv/blob/master/lib/refs/json-schema-v5.json",
    "type" : "object",
    "properties" : {
      "name" : { "type" : "string"},
      "description" : { "type" : "string"}
    },
    "required" : [ "name", "description" ]
  },
  "sync": {
    "ethereum-mainnet": {
      "protobuf_json": {
        "package": "Product",
        "messages": [{
          "name": "Product",
          "fields": [
            {"rule": "required", "type": "string", "name": "title", "id": 1},
            {"rule": "required", "type": "string", "name": "subtitle", "id": 2}
          ]
        }]
      }
    }
  }
  "registrant": {
    "name": "Apple",
    "description": "A technology company",
    "destination_blockchains": [{
      "blockchain_id": "ethereum-mainnet"
      "address":"0x123",
      "is_multi_access": "false"
    }]
  }
}
```
### Error Response

NotAuthorized:

```
HTTP/1.1 400 Bad Request
{
  "error": "10004",
  "message": "Not authorized to perform this operation",
  "detail": "Not Authorized. Entity 'Registry-SpecView', id 'n/a', permission 'read'."
}
```
## Create Spec

<p>Allows a user with access to an organization to create a Spec for that Organization. The Spec document is used to limit what kind of NewThings a user can create and determine how that NewThings is synchronized with the blockchain. The <code>json_schema</code> field is used to validate the structure of the <code>properties</code> field in all NewThings that are created with that Spec. The <code>sync</code> field is used to determine which blockchains the NewThings will be stored on. One option that can be seen below is the <code>ethereum</code> field. The `ethereum field must be supplied with a protobuf_json to encode that data and, if present, will automatically register the NewThings on the Ethereum blockchain</p>

	POST /api/1.0/registrant/:registrant_address/spec

### Headers

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| Authorization			| String			|  <p>The authorization token for the request. Should be in the format of &quot;Bearer $JWT&quot;</p>							|

### Parameters

| Name    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| registrant_address			| String			|  <p>The address of the registrant that the Schema belongs to</p>							|
| name			| String			|  <p>The name of the Schema registered (must be unique within your organization)</p>							|
| json_schema			| Object			|  <p>The JSON schema that identifies the properties that Things adhering to this Schema possess</p>							|
| sync			| Object			|  <p>An object which identifies which properties in the Schema specs will be synchronized with a blockchain</p>							|
| sync.ethereum-mainnet			| Object			| **optional** <p>An object for identifying properties on the Schema specs that will be synchronized with ethereum-mainnet. Each key in this object identifies the property in the Schema specs that is registered and the type represents that type of value it will be registered as</p>							|
| sync.ethereum-mainnet.protobuf_json			| Object			| **optional** <p>A protobuf object used to encode the data for ethereum-mainnet, must be provided if syncing with ethereum-mainnet</p>							|
| sync.quorum-local			| Object			| **optional** <p>An object for identifying properties on the Schema specs that will be synchronized with quorum-local. Each key in this object identifies the property in the Schema specs that is registered and the type represents that type of value it will be registered as</p>							|
| sync.quorum-local.protobuf_json			| Object			| **optional** <p>A protobuf object used to encode the data for quorum-local, must be provided if syncing with ethereum-mainnet</p>							|
| sync.hyperledger-iot-alliance			| Object			| **optional** <p>An object for identifying properties on the Schema specs that will be synchronized with hyperledger-iot-alliance. Each key in this object identifies the property in the Schema specs that is registered and the type represents that type of value it will be registered as</p>							|

### Examples

Example Usage for creating a Spec

```
curl --request POST \
--url http://discovery.chronicled.com/api/1.0/registrant/0xc257274276a4e539741ca11b590b9447b26a8051/spec \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer $JWT' \
--data '{
   "name": "product",
   "json_schema": {
     "type": "object",
     "properties": {
       "title": {"type": "string},
       "subtitle": {"type": "string}
     },
   },
   "sync": {
     "ethereum-mainnet": {
       "protobuf_json": {
         "package": "Product",
         "messages": [{
           "name": "Product",
           "fields": [
             {"rule": "required", "type": "string","name": "title","id": 1 }, 
             {"rule": "required", "type": "string","name": "subtitle", "id": 2}
           ]
         }]
       }
     }
   }
}'
```

### Success Response

Successful Response for creating a Spec 

```
{
  "__v":0,
  "updated_at":"2016-08-05T20:39:33.236Z",
  "__t":"SpecCreation",
  "_id":"avkMAvS",
  "created_at":"2016-08-05T20:39:33.234Z" 
  "spec": {
    "_id": "a31Ahz",
    "name": "product",
    "registrant_address": "0xc257274276a4e539741ca11b590b9447b26a8051",
    "json_schema": {
      "type": "object",
      "properties": {
        "title": {"type": "string"},
        "subtitle": {"type": "string"}
      },
    },
    "sync": {
      "ethereum-mainnet": {
        "protobuf_json": {
          "package": "Product",
          "messages": [{
            "name": "Product",
            "fields": [
              {"rule": "required", "type": "string", "name": "title", "id": 1},
              {"rule": "required", "type": "string", "name": "subtitle", "id": 2}
            ]
          }]
        }
      }
    }
  }
}
```
### Error Response

MissingProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The property 'json_schema' is missing"
 }
```
InvalidProperty

```
 HTTP/1.1 400 Bad Request

{
 "error":"10001",
 "message":"Provided parameter is invalid",
 "detail":"The value ${json_schema} for json_schema is not valid. Reason: The json_schema could not be compiled"
 }
```
NotAuthorized

```
 HTTP/1.1 400 Bad Request
{
 "error":"10004",
 "message":"Provided parameter is invalid",
    "detail": "Not authorized. Entity 'Spec', id 'n/a', permission 'create'."
 }
```


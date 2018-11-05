# [petstore-api.json](petstore-api.json)

The OpenAPI standard produces extremely nested data that can be hard to decipher. By using literate-json, you can document your API configuration in a much more readable way. This file produces the same example "Pet store API" example as found [here](https://github.com/OAI/OpenAPI-Specification/blob/master/examples/v3.0/petstore.yaml).

* openapi: 3.0.0
* info:
  * version: 1.0.0
  * title: Swagger petstore
  * license.name: MIT
* servers:
  1. url: http://petstore.swagger.io/v1

## paths

### /pets.get

Fetch data about pets currently in the database. Example curl command:

```sh
$ curl -X GET //pets_api/pets?limit=10
```

* summary: List all pets
* operationId: listPets
* tags: [pets]

#### parameters

1. ...
  * name: limit
  * in: query
  * description: How many items to return at one time (max 100)
  * required: false
  * schema:
    * type: integer
    * format: int32

#### responses

* 200:
  * description: A paged array of pets
  * headers.x-next:
    * description: A link to the next page of respones
    * schema.type: string
  * content.application/json.schema:
    * $ref: #/components/schemas/Pets
* default:
  * description: Unexpected error
  * content.application/json.schema:
    * $ref: #/componetns/schemas/Error

### /pets.post

Create a new pet entry in the database. Example:

```sh
$ CURL -X POST //pets_api/pets -d '{"name": "buster"}'
```

* summary: Create a pet
* operationId: createPets
* tags: [pets]

#### responses

* 201:
  * description: Null response
* default:
  * description: Unexpected error
  * content.application/json.schema:
    * $ref: #/components/schemas/Error


### /pets/{petId}.get

Fetch a single pet by its ID. Example:

```sh
$ curl -X GET //pets_api/pets/123
```

* summary: Info for a specific pet
* operationId: showPetById
* tags: [pets]

#### parameters

In the url, pass in the ID of the pet data you would like to fetch.

1. ...
  * name: petId
  * in: path
  * required: true
  * description: the id of the pet to retrieve
  * schema.type: string

#### responses

Returns the data for a single pet fetched by ID.

* 200:
  * description: Expected response to a valid request
  * content.application/json.schema:
    * $ref: #/components/schemas/Pets
* default:
  * description: Unexpected error
  * content.application/json.schema:
    * $ref: #/components/schemas/Error

## components.schemas 

All the reusable JSON schemas referenced in the API above.

### Pet

A single pet object.

* required: [id, name]
* properties:
  * id:
    * type: integer
    * format: int64
  * name.type: string
  * tag.type: string

### Pets

Multiple pets.

* type: array
* items:
  * $ref: #/components/schemas/Pet

### Error

A generic error response

* required: [code, message]
* properties:
  * code:
    * type: integer
    * format: int32
  * message.type: string

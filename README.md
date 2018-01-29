# GraphQL Binding for Swagger/OpenAPI

[![CircleCI](https://circleci.com/gh/graphql-binding/graphql-binding-openapi.svg?style=shield)](https://circleci.com/gh/graphql-binding/graphql-binding-openapi) [![npm version](https://badge.fury.io/js/graphql-binding-openapi.svg)](https://badge.fury.io/js/graphql-binding-openapi)

Embed any Swagger/OpenAPI endpoint as GraphQL API into your server application.

Both yaml and json specifications are supported.

## Install

```sh
yarn add graphql-binding-openapi
```

## Example

### Standalone
See [example directory](example) for a standalone example.

### With `graphql-yoga`
```js
const { OpenApi } = require('graphql-binding-openapi')
const { GraphQLServer } = require('graphql-yoga')

const resolvers = {
  Query: {
    findAvailablePets: async (parent, args, context, info) => {
      return context.petstore.query.findPetsByStatus({ status: "available" }, context, info)
    }
  }
}

const server = new GraphQLServer({ 
  resolvers, 
  typeDefs,
  context: async req => {
    ...req,
    petstore: await OpenApi.init('./petstore.json', 'http://petstore.swagger.io/v2')
  }
})

server.start(() => console.log('Server running on http://localhost:4000'))
```

## graphql-config support

OpenAPI bindings are supported in `graphql-config` using its extensions model. OpenAPI bindings can be added to the configuration like this:
```yaml
projects:
  petstore:
    schemaPath: src/generated/petstore.graphql
    extensions:
      openapi:
        definition: petstore.json
```
This will enable running `graphql get-schema` to output the generated schema from the definition to the defined schemaPath.

## Static bindings
Static binding support coming soon.


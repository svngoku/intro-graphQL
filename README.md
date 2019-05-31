# GraphQL 

It's a strongly typed query language and run time for your data .

* Open Soure and created by Facebook .

* Gives clients he power to describe exactly what data they want .

* Strongly typed ( think typescript for your data )

* Enables excellent developer tooling and experiences .

* Can sit in front of any existing API because its just a query language .

* Ecosystem is fantastic

# GraphQL vs REST

Different but sneakily similar

* GraphQL only has one URL . It does not need a resource url + verb combo . The request details are in POST body (sometimes GET) .

* in REST , the shape and size of the data resource is determined by the server , with GraphQL it's determined by the query (request) .

* In REST , you have to make multiple API calls to retrieve relational data, with GraphQL you can start with entry resource and traverse all the connections in one request .

* In REST the shape of the response is determined by whom ever created the server , in GraphQL the shap is determined by the query .

* In REST , a single request will execute on controller on the server , in GraphQL a request migth execute MANY resolvers on the server .

# GraphQL Basics & Starting the Server

## Apollo

It's basically a whole bunch of tooling and products around GraphQL .
Apollo Server is their implementation of creating GraphQL servers . Apollo Server work with express .

```javascript
    import { ApolloServer } from 'apollo-server';
```

## Schema

the root schema is the first part of the Schema , and this schema it's like a three , an 'Graph'

```javascript
const rootSchema = `
    type Cat {
        name: String
    }

    type Query {
        myCat : Cat
    }

    schema {
        query : Query
    }
`;
```

## Resolvers

A resolvers have a same functionality like a controller . He help us to resolve a query . In this example it is     ```Query myCat```.

```javascript
    const server = new ApolloServer({
        typeDefs: [rootSchema],
        resolvers: {
            Query : {
                myCat() {
                    return { name : 'Garfield'}
                }
            }
        }
    });
    ...
```

<!-- ![image]( =250x) -->

# GraphQL Schemas

By default, we already have a DB Schemas , and GraphQL schema strictly defines what resources , how they relate , and how a client and consume them .

* DB Schema is for keeping data consistent when entering our database .

* GraphQL schema is for defining what resources are available for querying , how they relate and how you can query them .

* Both schemas can be the same , or not . Database schema is a good starting point for your GraphQL schema .

* GraphQL schema sits in front of your database queries , it validates incoming request queries .

* Some GraphQL tools create GraphQL APIs based off of your database schemas , and the other way around .

## Schema Definition Language

### SDL

 Allow many ways to create schemas , SDL is the best , because it's a prefer way using by community and GraphQL docs .

* Schema Definition Language ( SDL ) .

* Instead of using functions to create a schema , use a verbose , string based syntax for your schemas . Later you can transform that syntax into many other representations if needed .

* Much easier to read .

* Can be composable .

* Supported by all tools .

## Scalar & Object types

Describe resources that will be used in queries and mutations.

* Scalar types are build in primitives
    - String
    - Int
    - Float
    - Boolean ( True or False)
    - ID ( references to a unique id , like a primary key in database)

* Object types are custom shapes with fields that either scalar types or other object types . We use the keyword ```type``` .

```typescript
    type Cat {
        name: String
        age: Int!
        bestFriend : Cat
    }
```

* Object types fields also describe any arguments and or validations.

* Types are the target or all requests .

The objective of a GraphQL , is to *resolve types* , and the objective of query os to get the types in the formatted way that you describe , that's allowed to you by the schema .

* Schema cheatsheet

![graphql](https://raw.githubusercontent.com/sogko/graphql-shorthand-notation-cheat-sheet/master/graphql-shorthand-notation-cheat-sheet.png)

## Query & Mutation types

Create, Read , Update and Delete  on your GraphQL API.

* Query type describes the different queries your API is capable of ( getOne and getMany)

* A query operation is just a name , with possible arguments that eventually returns type .

* A mutation is the same thing but with the intent of mutating the DB vs just reading . Mutation is for update , create and delete . With mutations , the most past of time we need it to change something in a datababse .

* Queries and Mutations are what will be available to clients with your API , think of them as your routes .

The Queries and Mutations are your routes , they're what your API can do .

## Writing a Mutation Types

```typescript

input CatInput {
    name: String
    age: Int!
    bestFriend : Cat!
}

type Mutation {
    newCat(input: CatInput!): Cat!
}

```

# GraphQL Resolvers

Like controllers , but instead resolve types all the way down .

* Resolvers are like controllers in a REST API . They are responsible for retrieving data .

* Every query and mutation your schema has , must have a resolver that treturns the specified type .

* Types and fields on types often have resolvers as well .

* Incoming query dictates what resolvers run and in what order .

## Creating resolvers

Return the same shap as described in the schema , or delegate to another resolver .

* Resolvers take a few args :
    * starting object (what the parent resolver returned or starting value from server).

    * args ( any arguments from the incoming request )

    * context ( shared context object across all resolvers , like the req object in express)

    * info ( advanced AST of the incoming request )
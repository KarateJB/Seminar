# Problems on Restful

1. Route will be more as API grows
2. APIs should be documented
3. Response payload is not expected for front-end
4. Many endpoints


# Stoplight

- API document
- Can check API payload schemas
- Must be maintain (Forget to update when API schema changes)

# GRAPHQL (by Facebook)

| GraphQL | Restful |
|:-------:|:-------:|
| Query | GET |
| mutation | PUT/POST/PATCH |


1. fragment
2. One endpoints


# GrapghiQL

- Schema as document
- Can query online

# Comparison

| | GraphQL | Restful |
| Endpoint | One | Many |
| Request # | Few | Many |
| Document |Schema as document | 3rd toolkit |


# Redux

> Stacks: React + Redux + Redux-Saga

## Restful

Action -> Saga -> Restful API -> Action -> Reducer (Update state)

> Actions,Saga will grows as APIs grows 

## GraphQL

Action -> Saga -> GraphQL -> Reducer (Update state)


# Apollo

1. Support React, Vuejs, Serverside...
2. Facade the GraphQL, replace the Redux-Saga, actions 
3. Include Request module
4. Cache
   - cache-first
   - cache-and-network: display cache and query at the same time
   - network-only
   - cache-only
   - no-cache 

### Query

```
<Query query={queryUser} fetchPolicy='cache-first'>
</Query>
```

### Mutation

```
<Mutation query={createUser} variables={{userId:'xxx'}}>
</Mutation>
```
OR

```
<Mutation query={createUser} variables={{userId:'xxx'}}>
   { 
     mutate => { 
      const variables = {userId:'xxxx'}
      //...
    }
   }
</Mutation>
```

### Reftech after Mutation

```
<Mutation query={createUser} variables={{userId:'xxx'}} refetchQUeries={refetchSomething}>
</Mutation>
```

> Notice that Appolo will update the cache after refetching data

### Cache

- Update cache manually
- Read cache


## Stacks

- graphql-tag
- react-apollo


# Hooks


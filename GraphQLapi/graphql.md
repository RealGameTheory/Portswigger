
#### Universal queries

If we send `query{__typename}` to any GraphQL endpoint, it will include the string `{"data": {"__typename": "query"}}` somewhere in its response. This is known as a universal query. This is very useful for probing graphql api.The query works because every GraphQL endpoint has a reserved field called `__typename` that returns the queried object's type as a string.


#### Common Endpoints
* `/graphql`
* `/api`
* `/api/graphql`
* `/graphql/api`
* `/graphql/graphql`

If these common endpoints don't return a GraphQL response, you could also try appending `/v1` to the path.
> **Note**: GraphQL services will often respond to any non-GraphQL request with a "query not present" or similar error.

#### Exploiting unsanitized arguments
**IDOR**:
For example, the query below requests a product list for an online shop:

```
    #Example product query

    query {
        products {
            id
            name
            listed
        }
    }
```
The product list returned contains only listed products.

```
    #Example product response

    {
        "data": {
            "products": [
                {
                    "id": 1,
                    "name": "Product 1",
                    "listed": true
                },
                {
                    "id": 2,
                    "name": "Product 2",
                    "listed": true
                },
                {
                    "id": 4,
                    "name": "Product 4",
                    "listed": true
                }
            ]
        }
    }
```
From this we can see that the product id 3 is not listed as it may have been delisted.

We can view the id 3 product by :
```
 query {
        product(id: 3) {
            id
            name
            listed
        }
    }
```
This query will give the respons with the details of the product id 3.

#### Discovering schema information

In this we have to map out the underlying schema by testing the api. There is an in-built tool called introspection in GrapgQL itself which will help get information about the schema. We can use the query `__schema` field. This field is available on the root type of all queries.

```
    #Introspection probe request

    {
        "query": "{__schema{queryType{name}}}"
    }
```
This query can be used to get the details about all the available queries
>**Note:** Burp Scanner can automatically test for introspection during its scans. If it finds that introspection is enabled, it reports a "GraphQL introspection enabled" issue.

The following query gives the full details about on queries, mutations, subscriptions, types, and fragments.
```
    #Full introspection query

    query IntrospectionQuery {
        __schema {
            queryType {
                name
            }
            mutationType {
                name
            }
            subscriptionType {
                name
            }
            types {
             ...FullType
            }
            directives {
                name
                description
                args {
                    ...InputValue
            }
            onOperation  #Often needs to be deleted to run query
            onFragment   #Often needs to be deleted to run query
            onField      #Often needs to be deleted to run query
            }
        }
    }

    fragment FullType on __Type {
        kind
        name
        description
        fields(includeDeprecated: true) {
            name
            description
            args {
                ...InputValue
            }
            type {
                ...TypeRef
            }
            isDeprecated
            deprecationReason
        }
        inputFields {
            ...InputValue
        }
        interfaces {
            ...TypeRef
        }
        enumValues(includeDeprecated: true) {
            name
            description
            isDeprecated
            deprecationReason
        }
        possibleTypes {
            ...TypeRef
        }
    }

    fragment InputValue on __InputValue {
        name
        description
        type {
            ...TypeRef
        }
        defaultValue
    }

    fragment TypeRef on __Type {
        kind
        name
        ofType {
            kind
            name
            ofType {
                kind
                name
                ofType {
                    kind
                    name
                }
            }
        }
    }
```
>**Note:** If introspection is enabled but the above query doesn't run, try removing the onOperation, onFragment, and onField directives from the query structure. Many endpoints do not accept these directives as part of an introspection query, and you can often have more success with introspection by removing them.

>**Note:** Reading the data given by the above query will be very hard to do. Hence, we have to view the relationships between schema entities using a GraphQL visualizer.

#### Suggestions

Even if introspection is entirely disabled, we can sometimes use suggestions to glean information on an API's structure.

Suggestions are a feature of the Apollo GraphQL platform in which the server can suggest query amendments in error messages. These are generally used where a query is slightly incorrect but still recognizable (for example, `There is no entry for 'productInfo'. Did you mean 'productInformation' instead?`).

Clairvoyance is a tool that uses suggestions to automatically recover all or part of a GraphQL schema, even when introspection is disabled.

# Discovering GraphQL #

Through this article, we are going to see what _GraphQL_ is and how we can use it. It is going to be a really simple introduction to get our hands on this tool.

## Introduction ##

_REST_ has become the standard for exposing data from a server and designing web _APIs_. It undeniably offers good advantages and functionalities.

Unfortunately, client applications rarely stay simple. Each one has specific requirements and thus needs different data from our system. To satisfy that need, we will add endpoints or add nested objects and probably deliver irrelevant data. Working and maintaining multiple endpoints could be difficult and as a platform grows, the number of endpoints will increase. Clients will need to ask for data from different endpoints increasing the number of _HTTP requests_. They will get too much or not enough information. They won't have exactly what they asked for.

_GraphQL_ offers a solution to these problems.

## What is GraphQL? ##

_GraphQL_ is a _query language_ and a server-side runtime for executing queries. GraphQL isn't tied to any specific database or storage engine. It was created by _Facebook_. It gives clients the power to ask for exactly what they need, nothing more, nothing less.

While typical _REST APIs_ require loading from multiple _URLs_, _GraphQL APIs_ get all the data we need in a single request.

_GraphQL APIs_ are organized in terms of types and fields, not endpoints. _GraphQL_ uses types to ensure applications only ask for what is possible and provide clear and helpful errors.

The _GraphQL_ layer takes place between the client and one or more data sources. It receives the requests of the client and fetches the necessary data according to the instructions received.

Many different programming languages support _GraphQL_. Here, we are going to use _JavaScript_ with _Node.js_.

## Basic terminology ##

Before getting our hands dirty, let's have an overview of the different terms related to the components of _GraphQL_.

### Query ###

A _Query_ is a read-only operation made against a _GraphQL Server_. They are similar to _GET_ requests and they allow to ask the server for the data that a client needs.

We write a _GraphQL Query_ as we would declare a _JSON object_. _GraphQL Queries_ support nested fields. In return, clients will have a _JSON_ result.

### Mutation ###

A _Mutation_ is a read-write operation requested to a _GraphQL Server_. In _REST APIs_, they would correspond to _POST_, _DELETE_ and _PUT_.

### Resolver ###

A _Resolver_ is a link between an operation and the code responsible for handling it. It provides a way to interact with databases through different operations. It tells _GraphQL_ how and where to fetch the data corresponding to a given field.

### Type ###

A _Type_ defines the shape of response data that can be returned from the _GraphQL Server_.

### Scalar ###

A _Scalar_ is a primitive _Type_, such as a _String_, _Int_, _Boolean_ or _Float_.

### Field ###

A _Field_ is a unit of data we can retrieve from an object.

### Argument ###

An _Argument_ is a set of key-value pairs attached to a specific field. _Arguments_ can be of many different types.

### Input ###

_Input Types_ look exactly the same as regular object types. They define the shape of input data that is sent to a _GraphQL Server_.

### Schema ###

A _Schema_ manages queries and mutations, defining what is allowed to be executed in the _GraphQL Server_. It defines what each _Query_ or _Mutation_ takes as input, and what each query or mutation returns as output. It describes the complete set of possible data that a client can access.

### Interface ###

An _Interface_ is an abstract type that includes a certain set of _Fields_ that a _Type_ must include to implement the _Interface_. It stores the names of the _Fields_ and their _Arguments_, so _GraphQL_ objects can inherit from it.

### Implementation ###

A _GraphQL Schema_ may use the term implements to define how an object inherits from an _Interface_.

### Connection ###

A _Connection_ allows to query related objects as part of the same call.

### Node ###

_Node_ is a generic term for an object.

### Edge ###

An _Edge_ represents connections between nodes. Every _Edge Field_ has a _Node Field_ and a _Cursor Field_. _Cursors_ are used for pagination.

## A simple example ##

Now we saw some theoretical notions, let's try to put them into practice by making a very straightforward example. As we said earlier, we are going to use _JavaScript_, _Node.js_ and _Express.js_ to reach our goal.

We are going to make a really simple _API_ that returns us the name of a band and its albums when we pass the band's id to the main query. We are going to use _MusicGraph API_ for this.

In the end, we are going to be able to make a request like so:

    {
      artist(id: "string) {
          name,
          albums {
          title,
          format,
          year
          }
      }
    }

Let's create a _package.json_ file like so:

    {
      "name": "simple-app",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
          "start": "node ./index.js",
          "watch": "nodemon --exec npm start"
      },
      "nodemonConfig": {
          "ignore": [
          "test/*",
          "docs/*"
          ],
          "delay": "2500"
      },
      "author": "",
      "license": "ISC",
      "dependencies": {
          "dotenv": "^4.0.0",
          "express": "^4.16.2",
          "express-graphql": "^0.6.11",
          "graphql": "^0.12.3",
          "node-fetch": "^1.7.3"
      },
      "devDependencies": {
          "nodemon": "^1.14.7"
      }
    }
<small>_package.json file_</small>

As we can see, in our dependencies, we have _Express.js_, as we said, but also _Express GraphQL_ and _GraphQL.js_. _Express GraphQL_ will help us to create a _GraphQL HTTP Server_ with _Express_ while _GraphQL.js_ is a reference implementation of _GraphQL_ for _JavaScript_.

We also have some dependencies like _Dotenv_ that lets us load environment variables from a _.env file_, _node-fetch_ that brings _window.fetch_ to _Node.js_ and _nodemon_ that watches for any changes in our application.

Our _index.js_ file is going to be filled with the following code:

    const express = require('express')
    const graphqlHTTP = require('express-graphql')
    const app = express()

    const schema = require('./schema')

    app.use('/graphql', graphqlHTTP({
      schema,
      graphiql: true
    }))

    app.listen(4000)
    console.log('Now listening to port 4000...')
<small>_index.js file_</small>

As we can notice here, we create our server and mount _Express GraphQL_ as a route handler. We also set the _graphiql_ option to true. It means that _GraphiQL_, which is an _in-browser IDE_ for exploring _GraphQL_, will be presented to us when the _GraphQL_ endpoint is loaded in a browser.

There is also a mention of a file called _schema.js_, which, obviously, represents our _Schema_. Let's take a look at it!

    const fetch = require('node-fetch')

    require('dotenv').config()

    const {
      GraphQLSchema,
      GraphQLObjectType,
      GraphQLInt,
      GraphQLString,
      GraphQLList
    } = require('graphql')

    // Defining the Album field
    const AlbumType = new GraphQLObjectType({
      name: 'Album',
      description: 'Album type',

      // Defining Fields
      fields: () => ({
        title: {
          type: GraphQLString,
          resolve: json => json.title
        },
        format: {
          type: GraphQLString,
          resolve: json => json.product_form
        },
        year: {
          type: GraphQLInt,
          resolve: json => json.release_year
        }
      })
    })

    // Defining the Artist field
    const ArtistType = new GraphQLObjectType({
      name: 'Artist',
      description: 'Artist type',

      // Defining the Fields
      fields: () => ({
        name: {
          type: GraphQLString,

          // Name Field resolver
          resolve: json => json.data.name
        },
        albums: {

          // Defining a GraphQLList filled with object AlbumType
          type: new GraphQLList(AlbumType),

          // Albums Field resolver

          resolve: (parent, args) => fetch(
              `http://api.musicgraph.com/api/v2/artist/${parent.data.id}/albums?api_key=${process.env.API_KEY}&limit=100`
            )
            .then(data => data.json())
            .then(json => json.data.filter(d => d.product_form === "album"))
        }
      })
    })

    // Defining our Schema

    module.exports = new GraphQLSchema({
      query: new GraphQLObjectType({
        name: 'RootQuery',
        description: 'The main query',

        // Defining the Fields
        fields: () => ({
          artist: {
            type: ArtistType,
            args: {
              id: { type: GraphQLString }
            },

            // Resolver - fetching data from MusicGraph API then returning a JSON object
            resolve: (root, args) => fetch(
              `http://api.musicgraph.com/api/v2/artist/${args.id}?api_key=${process.env.API_KEY}`
            )
            .then(res => res.json())
          }
        })
      })
    })
<small>_schema.js file_</small>

With the first lines we include a few things that we need from _GraphQL_.

Then, we define the _Album Field_ and the _Artist Field_. Finally, we define our _Schema_ before we export it.

Let's go to _localhost:4000/graphql_ and try the following query:

    {
      artist(id: "82047b8d-738e-4225-8ad9-76fe6f2a486c") {
        name
      }
    }

The result should be the following one:

    {
      "data": {
        "artist": {
          "name": "Depeche Mode"
        }
      }
    }

Now, the next query:

    {
      artist(id: "82047b8d-738e-4225-8ad9-76fe6f2a486c") {
        name,
        albums {
          title,
          format,
          year
        }
      }
    }

And the result should be:

    {
      "data": {
        "artist": {
          "name": "Depeche Mode",
          "albums": [
            {
              "title": "Depeche Mode -  Singles Box 5",
              "format": "album",
              "year": 2016
            },
            {
              "title": "Songs of Faith and Devotion: Live",
              "format": "album",
              "year": 2015
            },
            {
              "title": "Live in Berlin Soundtrack",
              "format": "album",
              "year": 2014
            },
            {
              "title": "Songs of Faith and Devotion (Remastered)",
              "format": "album",
              "year": 2013
            }
            ...
          ]
        }
      }
    }

## A little more ##

### Mutation ###

Of course, in our example, we didn't play with a _Mutation_ because it is not really appropriate. However, a _Mutation_ could be set up this way:

    export default new GraphQLSchema({
      query: QueryType,
      mutation: MutationType
    })

    const MutationType = new GraphQLObjectType({
      name: 'Item Mutations',
      description: 'A simple mutation',
      fields: () => ({
        ...itemMutations
      })
    })

    const itemMutations = {
      addItem: {
        type: ItemType,
        args: {
          input: {
            type: new GraphQLNonNull(ItemInputType),
          }
        }
      },
      resolve: (root, {input}) => {
        const newItem = await new Promise((resolve) => {
          setTimeout(() =>
            resolve(Object.assign(input, {
              id: random.uuid(),
            })), 100);
        })
        return newItem
      }
    }

    const ItemInputType = new GraphQLInputObjectType({
      name: 'ItemInputType',
      fields: () => ({
        name: {
          type: GraphQLString,
        },
      }),
    }) 

And it could be used like so:

    mutation {
      addItem(input: {
        name: "Foo"
      }) {
        id,
        name
      }
    }

### Subscriptions ###

_Subscriptions_ are a way for the server to push data itself to interested clients when an event happens. This is how _GraphQL_ deals with real-time communication.

To achieve this, we need _GraphQL subscriptions_ to implement subscriptions in _GraphQL_ and _subscriptions-transport-ws_ that works with a _WebSocket connection_, which will remain open between the server and subscribed clients.

First, we need to create a _PubSub instance_ like so:

    import { PubSub } from 'graphql-subscriptions'
    const pubsub = new PubSub()

Then we could do the following thing:

    const ITEM_ADDED_TOPIC = 'newItem';

    export default new GraphQLSchema({
      query: QueryType,
      mutation: MutationType,
      subscription: SubscriptionType
    })

    const MutationType = new GraphQLObjectType({
      name: 'Item Mutations',
      description: 'A simple mutation',
      fields: () => ({
        ...itemMutations
      })
    })

    const itemMutations = {
      addItem: {
        type: ItemType,
        args: {
          input: {
            type: new GraphQLNonNull(ItemInputType),
          }
        }
      },
      resolve: (root, {input}) => {
        const newItem = await new Promise((resolve) => {
          setTimeout(() =>
            resolve(Object.assign(input, {
              id: random.uuid(),
          })), 100)
        })
        pubsub.publish(ITEM_ADDED_TOPIC, { itemAdded: newItem })
        return newItem
      }
    }

    const SubscriptionType = {
      itemAdded: { 
        subscribe: () => pubsub.asyncIterator(ITEM_ADDED_TOPIC)
      }
    }

We now have to set up the server like so:

    import { execute, subscribe } from 'graphql'
    import { createServer } from 'http'
    import { SubscriptionServer } from 'subscriptions-transport-ws'

    const server = express()

    const ws = createServer(server)
    ws.listen(PORT, () => {
      console.log(`GraphQL Server is now running on http://localhost:${PORT}`)

      new SubscriptionServer({
        execute,
        subscribe,
        schema
      }, {
        server: ws,
        path: '/subscriptions',
      })
    })

    server.use('/graphiql', graphiqlExpress({
      endpointURL: '/graphql',
      subscriptionsEndpoint: `ws://localhost:${PORT}/subscriptions`
    }))

### Caching ###

Caching could be achieve with _Dataloader_. It will avoid unnecessary multiple requests.

Using _Dataloader_ could be achieved like so:

    ...
    const DataLoader = require('dataloader')
    ...

    const fetchArtist = id => fetch(`http://api.musicgraph.com/api/v2/artist/${args.id}?api_key=${process.env.API_KEY}`)
                              .then(res => res.json())

    app.use('/graphql', graphqlHTTP(req => {

      const artistLoader = new DataLoader(keys => Promise.all(keys.map(fetchArtist)))

      return {
        schema,
        context: {
          artistLoader
        }
        graphiql: true
      }

    }))

And then...

    module.exports = new GraphQLSchema({
      query: new GraphQLObjectType({
        name: 'RootQuery',
        description: 'The main query',

        // Defining the Fields
        fields: () => ({
          artist: {
            type: ArtistType,
            args: {
              id: { type: GraphQLString }
            },

            resolve: (root, args, context) => context.artistLoader.load(args.id)
          }
        })
      })
    })

## Conclusion ##

Through this small article, we took a look at _GraphQL_ and we got our hands on some theoretical and practical notions. We saw that _GraphQL_ is a _query language_ and how to quickly set up simple _GraphQL Server_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
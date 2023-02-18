# Getting started with graphQL subscriptions

GraphQL is a powerful and flexible query language for APIs that allows clients to specify exactly what data they need. One of the lesser-known features of GraphQL is subscriptions, which allow for real-time updates to be pushed from the server to the client.

Here's an example of how you can set up a GraphQL subscription using the `subscription` type in your schema:

```graphql
type Subscription {
  newMessage(roomId: ID!): Message!
    @resolve(fn: "newMessageResolver")
}

type Message {
  id: ID!
  text: String!
  room: Room!
}

type Room {
  id: ID!
  name: String!
}
```

In this example, the `newMessage` subscription is defined as a field on the `Subscription` type. It takes a single argument, `roomId`, and returns a `Message` type. The `@resolve` directive is used to specify the function that will be used to resolve the subscription.

To actually subscribe to the `newMessage` subscription, you can use a GraphQL client library, such as Apollo Client or Relay, to open a websocket connection to the server and send a subscription query. Here's an example using Apollo Client:

```javascript
const subscription = gql`
  subscription newMessage($roomId: ID!) {
    newMessage(roomId: $roomId) {
      id
      text
      room {
        id
        name
      }
    }
  }
`;

const observer = client.subscribe({
  query: subscription,
  variables: { roomId: "123" }
});

observer.subscribe({
  next: data => console.log(data)
});
```

With this code, the client will open a websocket connection to the server and subscribe to the `newMessage` subscription, passing the `roomId` variable as an argument. The server will push new messages to the client in real-time as they become available, and the `next` callback will be called with the updated data.

In conclusion, GraphQL subscriptions allow for real-time updates to be pushed from the server to the client, making it easy to build dynamic and responsive web applications. Whether you're building a chat app, a real-time dashboard, or any other type of application that requires real-time updates, GraphQL subscriptions are a great tool to have in your toolbox.
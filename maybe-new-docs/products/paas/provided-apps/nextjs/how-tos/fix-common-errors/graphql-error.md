# رفع خطای GET query missing در GraphQL

قابلیت Playground در Apollo Server در محیط Production به‌صورت پیش‌فرض غیرفعال است. برای فعال‌سازی، لازم است تا فیلدهای `introspection` و `playground` را برابر با  `true` تنظیم و سپس دیپلوی کنید:

```js
const { ApolloServer } = require('apollo-server');
const { typeDefs, resolvers } = require('./schema');

const server = new ApolloServer({
  typeDefs,
  resolvers,
  introspection: true,
  playground: true,
});

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```
برای اطلاعات بیشتر به [مستندات رسمی ApolloServer](https://www.apollographql.com/docs/apollo-server/testing/graphql-playground/#enabling-graphql-playground-in-production) مراجعه کنید.
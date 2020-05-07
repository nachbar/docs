With the basic setup complete, next you will model the data your application will store. You'll use Amplify DataStore to persist your application's data. DataStore provides a programming model for storing data offline and online without writing additional code. DataStore models are specified as [GraphQL](http://graphql.org/) schemas. If you'd like, first [learn more](~/cli/graphql-transformer/overview.md) about annotating GraphQL schemas and data modeling.

1. Switch to **Project** view in Android Studio and open the schema file at `amplify/backend/api/amplifyDatasource/schema.graphql`.  

    In this file, paste the following schema:

    ```graphql
    type Todo @model {
      id: ID!
      name: String!
      priority: Int
      description: String
    }
    ```

    This creates a model called `Todo` with four properties:

    - **ID** an auto-generated identifier field for an item
    - **name** a non-optional `String` field that serves as the name of an item
    - **priority** an optional `Int` field that indicates the importance of an item
    - **description** an optional `String` field that holds more information about an item

1. Next, generate the classes for these models. In Android Studio, click the Gradle Task dropdown in the toolbar and select **modelgen**. Run the task. When complete, you will have generated classes under `app/src/main/java/com/amplifyframework.datastore.generated.model`.

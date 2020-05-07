Now that your have DataStore persisting data locally, in the next step you'll connect it to the cloud. With a couple of commands, you'll create an AWS AppSync API and configure DataStore to synchronize its data to it.

1. Edit your `onCreate` method to remove the item creation and save. Your `onCreate()` should only include the code required to initiatize Amplify and not calls to `Todo.builder()` or `Amplify.DataStore.save()`.

1. Below the initialization code, add the following: 

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
  Todo item = Todo.builder()
    .name("Tidy up the office")
    .description("Organize books, vacuum, take out the trash")
    .build();

    Amplify.DataStore.save(
            item,
            success -> Log.i("Tutorial", "Saved item: " + success.item.getName()),
            error -> Log.e("Tutorial", "Could not save item to DataStore", error)
    );
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  val item: Todo = Todo.builder()
        .name("Tidy up the office")
        .description("Organize books, vacuum, take out the trash")
        .build()

   Amplify.DataStore.save(
          item,
          { success -> Log.i("Tutorial", "Saved item: " + success.item.name) },
          { error -> Log.e("Tutorial", "Could not save item to DataStore", error) }
  )
  ```

  </amplify-block>
  </amplify-block-switcher>

1. Open up your terminal and go to your project directory. Run `amplify init`. Enter a name for your environment such as *tutorial*. Select your preferred editor, and your AWS profile:

    ```console
    ? Enter a name for the environment: tutorial
    ? Choose your default editor: Vim (via Terminal, Mac OS only)
    Using default provider  awscloudformation
    ? Do you want to use an AWS profile? Yes
    ? Please choose the profile you want to use: amplify
    ```

1. In the terminal, run `amplify push`. You can select the defaults to the prompts:

    ```console
    ? Do you want to generate code for your newly created GraphQL API Yes
    ? Enter the file name pattern of graphql queries, mutations and subscriptions app/src/main/graphql/**/*.graphql
    ? Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions Yes
    ? Enter maximum statement depth [increase from default if your schema is deeply nested] 2
    ```

1. Open the Amplify configuration file `app/src/main/res/raw/amplifyconfiguration.json` in a text editor. Configure the DataStore plugin to synchronize with the backend API. Add the `plugins` object to the `dataStore` key:

    ```json
    {
        "dataStore": {
            "plugins": {
                "awsDataStorePlugin": {
                    "syncMode": "api"
                }
            }
        },
        "api: {
            ...
        }
    }
    ```

1. Run your application. This will create a new Todo and synchronize it to your API.

1. In the terminal, run `amplify api console`. When prompted, select **GraphQL**. This will open the AWS AppSync console.

1. Select **queries**.

1. Enter the following query:

    ```graphql
    query GetPost {
      listTodos {
        items {
          id
          name
          description
        }
      }
    }
    ```

1. Hit the Play button to run the query. This will return all of the synchronized Todos:

    ```json
    {
      "data": {
        "listTodos": {
        "items": [
            {
              "id": "ce5ac1c7-4ff8-48d2-90bc-f5ff63fb8c9a",
              "name": "Tidy up the office",
              "description": "Organize books, vacuum, take out the trash"
            }
          ]
        }
      }
    }
    ```

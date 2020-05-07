Next you'll use the generated Todo model to create, update, query, and delete data. In this section you'll initialize DataStore, and then manipulate Todo items.

## Configure Amplify and DataStore

First, we'll add the DataStore plugin and configure Amplify. Typically, a good place to do this is in the [`onCreate()`](https://developer.android.com/reference/android/app/Application#onCreate()) method of [Android's `Application` class](https://developer.android.com/reference/android/app/Application). For the purposes of this tutorial, you can place these examples in the `onCreate()` method of the `MainActivity` class.

1. Open `MainActivity` and add the following code to the bottom of the `onCreate()` method:

  <amplify-block-switcher>
  <amplify-block name="Java">
  
  ```java
  ModelProvider modelProvider = AmplifyModelProvider.getInstance();

  try {
      Amplify.addPlugin(AWSDataStorePlugin.forModels(modelProvider));
      Amplify.configure(getApplicationContext());

      Log.i("Tutorial", "Initialized Amplify");
  } catch (AmplifyException e) {
      Log.e("Tutorial", "Could not initialize Amplify", e);
  }
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  val modelProvider: ModelProvider = AmplifyModelProvider.getInstance()

  try {
      Amplify.addPlugin(AWSDataStorePlugin.forModels(modelProvider))
      Amplify.configure(applicationContext)
      Log.i("Tutorial", "Initialized Amplify")
  } catch (e: AmplifyException) {
      Log.e("Tutorial", "Could not initialize Amplify", e)
  }
  ```

  </amplify-block>
  </amplify-block-switcher>

1. Run the application. In logcat, you'll see a log line indicating success:

    ```console
    com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
    ```

    To make this easier to find, enter the string "Tutorial" into the search field.

## Create a Todo

Next, you'll create a Todo and save it to DataStore.

1. Open `MainActivity` and add the following code to the bottom of the `onCreate()` method:

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
  Todo item = Todo.builder()
    .name("Build Android application")
    .description("Build an Android Application using Amplify")
    .build();
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  val item: Todo = Todo.builder()
        .name("Build Android application")
        .description("Build an Android Application using Amplify")
        .build()
  ```

  </amplify-block>
  </amplify-block-switcher>

  This code creates a Todo item with two properties: a title and a description. This is a plain object that isn't stored in DataStore yet.

1. Below that, add the code to save the item to DataStore:

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
    Amplify.DataStore.save(
            item,
            success -> Log.i("Tutorial", "Saved item: " + success.item.getName()),
            error -> Log.e("Tutorial", "Could not save item to DataStore", error)
    );
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  Amplify.DataStore.save(
          item,
          { success -> Log.i("Tutorial", "Saved item: " + success.item.name) },
          { error -> Log.e("Tutorial", "Could not save item to DataStore", error) }
  )
  ```

  </amplify-block>
  </amplify-block-switcher>

1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see an indication that the item was saved successfully:

  ```console
  com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
  com.amplifyframework.examples.todo I/Tutorial: Saved item: Build application
  ```

1. Edit your code to change the item to save an additional item. Let's change the name and description, and add a priority:

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
  Todo item = Todo.builder()
    .name("Finish quarterly taxes")
    .priority(1)
    .description("Taxes are due for the quarter next week")
    .build();
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  val item = Todo.builder()
        .name("Finish quarterly taxes")
        .priority(1)
        .description("Taxes are due for the quarter next week")
        .build()
  ```

  </amplify-block>
  </amplify-block-switcher>

1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see an indication that the item was saved successfully:

  ```console
  com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
  com.amplifyframework.examples.todo I/Tutorial: Saved item: Finish quarterly taxes
  ```

## Query Todos

Now that you have some data in DataStore, you can run queries to retrieve those records.

1. Edit your `onCreate` method to remove the item creation and save. Your `onCreate()` should only include the code required to initiatize Amplify and not calls to `Todo.builder()` or `Amplify.DataStore.save()`.

1. Below the initialization code, add the following:

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
  Amplify.DataStore.query(
          Todo.class,
          todos -> {
              while (todos.hasNext()) {
                  Todo todo = todos.next();

                  Log.i("Tutorial", String.format("==== Todo (%s) ====", todo.getId()));
                  Log.i("Tutorial", "Name: " + todo.getName());
                  Log.i("Tutorial", "Description: " + todo.getDescription());

                  if (todo.getPriority() > 0) {
                      Log.i("Tutorial", "Priority: " + todo.getPriority().toString());
                  }
              }
          },
          failure -> Log.e("Tutorial", "Could not query DataStore", failure)
  );
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  Amplify.DataStore.query(
        Todo::class.java,
        { todos ->
            while (todos.hasNext()) {
                val todo: Todo = todos.next()
                val id: String = todo.id;
                val name: String = todo.name;
                val description: String? = todo.description
                val priority: Int? = todo.priority

                Log.i("Tutorial", "==== Todo ($id) ====")
                Log.i("Tutorial", "Name: $name")
                Log.i("Tutorial", "Description: $description")

                if (priority != null) {
                    Log.i("Tutorial", "Priority: $priority")
                }
            }
        },
        { failure -> Log.e("Tutorial", "Could not query DataStore", failure) }
  )
  ```

  </amplify-block>
  </amplify-block-switcher>

1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see both items returned:

  ```console
  com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
  com.amplifyframework.examples.todo I/Tutorial: ==== Todo (d081b5d0-417b-4baf-8d9b-1d487a83b22b) ====
  com.amplifyframework.examples.todo I/Tutorial: Name: Build application
  com.amplifyframework.examples.todo I/Tutorial: Description: Build an Android Application using Amplify
  com.amplifyframework.examples.todo I/Tutorial: ==== Todo (1669ee9e-45f4-4563-b330-7b60ca43ccfb) ====
  com.amplifyframework.examples.todo I/Tutorial: Name: Finish quarterly taxes
  com.amplifyframework.examples.todo I/Tutorial: Description: Taxes are due for the quarter next week
  com.amplifyframework.examples.todo I/Tutorial: Priority: 1
  ```

1. Queries can also contain predicate filters. These will query for specific objects matching a certain condition.

  The following predicates are supported:

  **Strings**
  
  `eq` `ne` `le` `lt` `ge` `gt` `contains` `notContains` `beginsWith` `between`

  **Numbers**

  `eq` `ne` `le` `lt` `ge` `gt` `between`

  **Lists**

  `contains` `notContains`

  To use a predicate, pass an additional argument into your query. For example, to see all high priority items:

  <amplify-block-switcher>
  <amplify-block name="Java">

  ```java
  Amplify.DataStore.query(
          Todo.class,
          Todo.PRIORITY.eq(1),
          todos -> {
              while (todos.hasNext()) {
                  Todo todo = todos.next();

                  Log.i("Tutorial", String.format("==== Todo (%s) ====", todo.getId()));
                  Log.i("Tutorial", "Name: " + todo.getName());
                  Log.i("Tutorial", "Description: " + todo.getDescription());

                  if (todo.getPriority() > 0) {
                      Log.i("Tutorial", "Priority: " + todo.getPriority().toString());
                  }
              }
          },
          failure -> Log.e("Tutorial", "Could not query DataStore", failure)
  );
  ```

  </amplify-block>

  <amplify-block name="Kotlin">

    ```kotlin
    Amplify.DataStore.query(
        Todo::class.java,
        Todo.PRIORITY.eq(1),
        Consumer { todos ->
            while (todos.hasNext()) {
                val todo = todos.next()
                val id = todo.id;
                val name = todo.name;
                val description = todo.description
                val priority: Int? = todo.priority

                Log.i("Tutorial", "==== Todo ($id) ====")
                Log.i("Tutorial", "Name: $name")
                Log.i("Tutorial", "Description: $description")

                if (priority != null) {
                    Log.i("Tutorial", "Priority: $priority")
                }
            }
        },
        Consumer { failure -> Log.e("Tutorial", "Could not query DataStore", failure) }
    )
    ```

  </amplify-block>
  </amplify-block-switcher>

  In the above, the only difference is the addition of `Todo.PRIORITY.eq(1)` as the second argument.

1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see only the high priority item returned:

  ```console
  com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
  com.amplifyframework.examples.todo I/Tutorial: ==== Todo (1669ee9e-45f4-4563-b330-7b60ca43ccfb) ====
  com.amplifyframework.examples.todo I/Tutorial: Name: Finish quarterly taxes
  com.amplifyframework.examples.todo I/Tutorial: Description: Taxes are due for the quarter next week
  com.amplifyframework.examples.todo I/Tutorial: Priority: 1
  ```

## Update a Todo

You may want to change the contents of a record. Below, we'll query for a record, create a copy of it, modify it, and save it back to DataStore. 

1. Edit your `onCreate` method to remove the item creation and save. Your `onCreate()` should only include the code required to initiatize Amplify and not calls to `Todo.builder()` or `Amplify.DataStore.save()`.

1. Below the initialization code, add the following: 

  <amplify-block-switcher>
  <amplify-block name="Java">

    ```java
    Amplify.DataStore.query(
            Todo.class,
            Todo.NAME.eq("Finish quarterly taxes"),
            todos -> {
                while (todos.hasNext()) {
                    Todo todo = todos.next();
                    Todo updated = todo.copyOfBuilder()
                            .name("File quarterly taxes")
                            .build();

                    Amplify.DataStore.save(updated,
                            success -> Log.i("Tutorial", "Updated item: " + success.item.getName()),
                            error -> Log.e("Tutorial", "Could not save item to DataStore", error)
                    );
                }
            },
            failure -> Log.e("Tutorial", "Could not query DataStore", failure)
    );
    ```

  </amplify-block>

  <amplify-block name="Kotlin">

    ```kotlin
    Amplify.DataStore.query(
            Todo::class.java,
            Todo.NAME.eq("Finish quarterly taxes"),
            Consumer { todos ->
                while (todos.hasNext()) {
                    val todo = todos.next()
                    val updated = todo.copyOfBuilder()
                            .name("File quarterly taxes")
                            .build()
                    Amplify.DataStore.save(updated,
                            { success -> Log.i("Tutorial", "Updated item: " + success.item.name) },
                            { error -> Log.e("Tutorial", "Could not save item to DataStore", error) }
                    )
                }
            },
            Consumer { failure -> Log.e("Tutorial", "Could not query DataStore", failure) }
    )
    ```

  </amplify-block>
  </amplify-block-switcher>

    Models in DataStore are immutable, so in order to update you first create a copy of the object, modify its properties, and then save it.

1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see an indication that the item was updated successfully:

    ```console
    com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
    com.amplifyframework.examples.todo I/Tutorial: Updated item: File quarterly taxes
    ```
  
## Delete a Todo

To round out our CRUD operations, we'll query for a record and delete it from DataStore.

1. Edit your `onCreate` method to remove the item creation and save. Your `onCreate()` should only include the code required to initiatize Amplify and not calls to `Todo.builder()` or `Amplify.DataStore.save()`.

1. Below the initialization code, add the following: 

  <amplify-block-switcher>
  <amplify-block name="Java">

    ```java
    Amplify.DataStore.query(
        Todo.class,
        Todo.NAME.eq("File quarterly taxes"),
        todos -> {
            if (todos.hasNext()) {
                Todo todo = todos.next();

                Amplify.DataStore.delete(todo,
                        success -> Log.i("Tutorial", "Deleted item: " + success.item.getName()),
                        error -> Log.e("Tutorial", "Could not delete item", error)
                );
            }
        },
        failure -> Log.e("Tutorial", "Could not query DataStore", failure)
  );
  ```
  </amplify-block>

  <amplify-block name="Kotlin">

  ```kotlin
  Amplify.DataStore.query(
        Todo::class.java,
        Todo.NAME.eq("File quarterly taxes"),
        Consumer { todos ->
            if (todos.hasNext()) {
                val todo = todos.next()
                Amplify.DataStore.delete(todo,
                        { success -> Log.i("Tutorial", "Deleted item: " + success.item.name) },
                        { error -> Log.e("Tutorial", "Could not delete item", error) }
                )
            }
        },
        Consumer { failure -> Log.e("Tutorial", "Could not query DataStore", failure) }
  )
  ```

  </amplify-block>
  </amplify-block-switcher>

  1. Clear the logcat window by pressing the trashcan icon. Run the application. In logcat, you'll see an indication that the item was deleted successfully:

    ```console
    com.amplifyframework.examples.todo I/Tutorial: Initialized Amplify
    com.amplifyframework.examples.todo I/Tutorial: Deleted item: File quarterly taxes
    ```

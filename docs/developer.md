# Developer documentation

## Design

### Creating

- Creating the tree of a document:

    ```
    tsi-bash-util create tree [--name "<tree's name>"] ["<tree's content>"]
    ```

- Creating an attribute:

    ```
    tsi-bash-util create attribute "<attribute's name>" ["<attribute's value>"]
    ```

- Creating a referenced attribute:

    ```
    tsi-bash-util create attribute declaration "<attribute's name>"
    tsi-bash-util create attribute definition "<attribute's name>" ["<attribute's value>"]
    ```

- Creating a node:

    ```
    tsi-bash-util create node "<node's name>" ["<node's content>"]
    ```

- Creating a referenced node:

    ```
    tsi-bash-util create node declaration "<node's name>"
    tsi-bash-util create node definition "<node's name>" ["<node's content>"]
    ```

### Reading

- Get the content of an element in the tree of a document:

    ```
    tsi-bash-util get "<element's path>" "<tree>"
    ```

## Workflow

1. Write some code.
2. Write unit tests for this code.
3. Run unit tests for the whole application.
4. If all unit tests pass, write integration tests for this code.
5. Run integration tests for the whole application.
6. If all integration tests pass, commit.

## Code conventions

1. There is to be no comments in the source code.
2. Numbers **are** to be placed within double quotation marks except where it is not possible.
3. An exit code is to mean either an error or no error.
4. For a function, returning an empty string is to mean returning "false", and returning the string `yes` is to mean returning "true".
5. Paths are **not** to be placed within any quotation marks except where required.
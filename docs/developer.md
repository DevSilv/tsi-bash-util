# Developer documentation

## Design

### Creating

- Creating the document:

    ```
    tsi-bash-util create doc "<main tree>" [...branches]
    ```

    where `...branches` is a list of arguments (in the same sense as in the Bash manual); each argument represents a branch, and each branch represents the definition of a referenced element.

- Creating the main tree of a document:

    ```
    tsi-bash-util create tree [--name "<tree's name>"] ["<tree's content>"]
    ```

- Creating an attribute:

    ```
    tsi-bash-util create attr "<attribute's name>" ["<attribute's value>"]
    ```

- Creating a referenced attribute:

    ```
    # Declaration
    tsi-bash-util create attr dec "<attribute's name>"

    # Definition
    tsi-bash-util create attr def "<attribute's name>" ["<attribute's value>"]
    ```

- Creating a node:

    ```
    tsi-bash-util create node "<node's name>" ["<node's content>"]
    ```

- Creating a referenced node:

    ```
    # Declaration
    tsi-bash-util create node dec "<node's name>"

    # Definition
    tsi-bash-util create node def "<node's name>" ["<node's content>"]
    ```

### Reading

- Get the content of an element within a document:

    ```
    tsi-bash-util get "<element's path>" "<document>"
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
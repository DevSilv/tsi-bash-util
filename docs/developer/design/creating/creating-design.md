# Creating design

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
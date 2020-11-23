# Developer documentation

## Automated testing

### Environment requirements

The environment of one has to fulfil at least the following requirements so that one be able to run automated tests:

1. The shell is [Bash](https://www.gnu.org/software/bash/), version >= 5.0, and the path to its executable is `/bin/bash`.
2. There is available the `/dev/null` device file.
3. The following Linux shell utilities are available through the `PATH` environment variable:
    - find (GNU findutils), version >= `4.6.0`

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

- Get an element within a document:

    ```
    tsi-bash-util get "<element's path>" "<document>"
    ```

    Drafts of algorithms:

    - Getting a standard attribute (`(doc, std attr's path) -> std attr`):

        ```
            GET_STD_ATTR (doc, path, cur, res)
                if greater(cur, length(doc))
                then return NULL
                else
                    if empty(path)
                    then
                        if attr_value(doc[cur])
                        then return GET_STD_ATTR(
                            doc,
                            path,
                            cur,
                            concat(res, NEWLINE, doc[cur])
                        )
                        else return res
                    else
                        if equal(size(path), 1)
                        then
                            if equal("attr " + path[0], doc[cur])
                            then return GET_STD_ATTR(
                                doc,
                                tail(path),
                                cur + 1,
                                concat(res, doc[cur])
                            )
                            else return GET_STD_ATTR(doc, path, cur + 1, res)
                        else
                            if equal("node " + path[0], doc[cur])
                            then return GET_STD_ATTR(
                                doc,
                                tail(path),
                                cur + 1,
                                res
                            )
                            else return GET_STD_ATTR(doc, path, cur + 1, res)
        ```

    - Getting a standard node (`(doc, std node's path) -> std node`):

        ```
        GET_STD_NODE (doc, path, cur, level, res)
            if greater(cur, length(doc))
            then return NULL
            else
                if empty(path)
                then
                    if node_start(doc[cur])
                    then return GET_STD_NODE(
                        doc,
                        path,
                        cur + 1,
                        level + 1,
                        concat(res, doc[cur])
                    )
                    else
                        if node_end(doc[cur])
                        then
                            if equal(level, 0)
                            then return concat(res, doc[cur])
                            else return GET_STD_NODE(
                                doc,
                                path,
                                cur + 1,
                                level - 1,
                                concat(res, doc[cur])
                            )
                        else return GET_STD_NODE(
                            doc,
                            path,
                            cur + 1,
                            level,
                            concat(res, doc[cur])
                        )
                else
                    if equal("node " + path[0], doc[cur])
                        if equal(size(path), 1)
                        then return GET_STD_NODE(
                            doc,
                            tail(path),
                            cur + 1,
                            level,
                            concat(res, doc[cur])
                        )
                        else return GET_STD_NODE(
                            doc,
                            tail(path),
                            cur + 1,
                            level,
                            res
                        )
                    else return GET_STD_NODE(doc, path, cur + 1, level, res)
        ```

    - Getting a referenced attribute (`(doc, ref attr's path) -> ref attr`):

        ```
        // not empty(doc)
        // not empty(orig_path)
        // not empty(path)
        // equal(cur, 0)
        // equal(count, 0)
        // equal(res, "")
        GET_REF_ATTR (doc, orig_path, path, cur, count, res)
            if greater(cur, length(doc))
            then return NULL
            else
                if empty(path)
                then
                    if attr_value(doc[cur])
                    then return GET_STD_ATTR(
                        doc,
                        orig_path,
                        path,
                        cur + 1,
                        count,
                        concat(res, NEWLINE, doc[cur])
                    )
                    else return res
                else
                    if equal(size(path), 1)
                    then
                        if equal("attr " + path[0], doc[cur])
                        then
                            if equal(count, 0)
                            then return GET_STD_ATTR(
                                doc,
                                orig_path,
                                orig_path,
                                cur + 1,
                                count + 1,
                                res
                            )
                            else return GET_STD_ATTR(
                                doc,
                                orig_path,
                                tail(path),
                                cur + 1,
                                count,
                                concat(res, doc[cur])
                            )
                        else return GET_STD_ATTR(doc, path, cur + 1, res)
                    else
                        if equal("node " + path[0], doc[cur])
                        then return GET_STD_ATTR(
                            doc,
                            orig_path,
                            tail(path),
                            cur + 1,
                            count,
                            res
                        )
                        else return GET_STD_ATTR(
                            doc,
                            orig_path,
                            path,
                            cur + 1,
                            count,
                            res
                        )
        ```

    - Getting a referenced node (`(doc, ref node's path) -> ref node`):

        ```
        // not empty(doc)
        // not empty(orig_path)
        // not empty(path)
        // equal(cur, 0)
        // equal(level, 0)
        // equal(count, 0)
        // equal(res, "")
        GET_REF_NODE (doc, orig_path, path, cur, level, count, res)
            if greater(cur, length(doc))
            then return NULL
            else
                if empty(path)
                then
                    if node_start(doc[cur])
                    then return GET_STD_NODE(
                        doc,
                        orig_path,
                        path,
                        cur + 1,
                        level + 1,
                        count,
                        concat(res, doc[cur])
                    )
                    else
                        if node_end(doc[cur])
                        then
                            if equal(level, 0)
                            then return concat(res, doc[cur])
                            else return GET_STD_NODE(
                                doc,
                                orig_path,
                                path,
                                cur + 1,
                                level - 1,
                                count,
                                concat(res, doc[cur])
                            )
                        else return GET_STD_NODE(
                            doc,
                            orig_path,
                            path,
                            cur + 1,
                            level,
                            count,
                            concat(res, doc[cur])
                        )
                else
                    if equal("node " + path[0], doc[cur])
                    then
                        if equal(size(path), 1)
                        then
                            if equal(count, 0)
                            then return GET_STD_NODE(
                                doc,
                                orig_path,
                                orig_path,
                                cur + 1,
                                level,
                                count,
                                res
                            )
                            else GET_STD_NODE(
                                doc,
                                orig_path,
                                tail(path),
                                cur + 1,
                                level,
                                count,
                                res
                            )
                        else return GET_STD_NODE(
                            doc,
                            orig_path,
                            tail(path),
                            cur + 1,
                            level,
                            count,
                            res
                        )
                    else return GET_STD_NODE(
                        doc,
                        orig_path,
                        path,
                        cur + 1,
                        level,
                        count,
                        res
                    )
        ```

## Workflow

1. Write some code.
2. Write unit tests for this code.
3. Run unit tests for the whole application.
4. If all unit tests pass, write integration tests for this code.
5. Run integration tests for the whole application.
6. If all integration tests pass, commit.

## Code conventions

### Comments

- There is to be no comments in the source code.

### Quoting

- Numbers **are** to be placed within double quotation marks except where it is not possible.
- Paths are **not** to be placed within any quotation marks except where required.

### Exiting and returning

- An exit code is to mean either an error or no error.

### Type system

- For a function, returning an empty string is to mean returning "false", and returning the string `yes` is to mean returning "true".

### Naming

- Names of functions need not start with a verb.
- Names of functions are to be as short as possible.
- Names of functions are to be prefixed with `tsi_`.

## Best practices used

### Error handling

- Throw early, catch late. (Throw at the point that the error firstly occurs, catch at the point that the error may be appropriately handled, as I understood it.)
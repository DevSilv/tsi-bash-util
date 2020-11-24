# Attributes getting design draft

- Getting an attribute ("generic" attribute getting; `(doc, attr's path) -> attr`):

    ```
    // not empty(doc)
    // equal(prev_path, path)
    // not empty(path)
    // equal(cur, 0)
    // equal(count, 0)
    // equal(res, "")
    GET_ATTR (doc, prev_path, path, cur, count, res)
        if greater(cur, length(doc))
        then return NULL
        else
            if empty(path)
            then
                if attr_val(doc[cur])
                then return GET_ATTR (
                    doc,
                    prev_path,
                    path,
                    cur + 1,
                    count,
                    concat(res, doc[cur])
                )
                else return res
            else
                if prefixed(doc[cur], "attr " + path[0])
                then return GET_ATTR (
                    doc,
                    path,
                    tail(path),
                    cur + 1,
                    count,
                    concat(res, doc[cur])
                )
                else
                    if prefixed(doc[cur], "ref attr " + path[0])
                    then
                        if equal(count, 1)
                        then return GET_ATTR (
                            doc,
                            path,
                            tail(path),
                            cur + 1,
                            count,
                            concat(res, doc[cur])
                        )
                        else return return GET_ATTR (
                            doc,
                            prev_path,
                            prev_path,
                            cur + 1,
                            count + 1,
                            res
                        )
                    else
                        if equal(doc[cur], "node " + path[0])
                        then return GET_ATTR (
                            doc,
                            path,
                            tail(path),
                            cur + 1,
                            count,
                            res
                        )
                        else
                            if equal(doc[cur], "ref node " + path[0])
                            then 
                                if equal(count, 1)
                                then return GET_ATTR (
                                    doc,
                                    path,
                                    tail(path),
                                    cur + 1,
                                    count,
                                    res
                                )
                                else return GET_ATTR (
                                    doc,
                                    prev_path,
                                    prev_path,
                                    cur + 1,
                                    count + 1,
                                    res
                                )
                            else return GET_ATTR (
                                doc,
                                prev_path,
                                path,
                                cur + 1,
                                count,
                                res
                            )
    ```
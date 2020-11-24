# Nodes getting design draft

- Getting a node ("generic" node getting; `(doc, node's path) -> node`):

    ```
    // not empty(doc)
    // equal(prev_path, path)
    // not empty(path)
    // equal(cur, 0)
    // equal(level, 0)
    // equal(count, 0)
    // equal(res, "")
    GET_NODE (doc, prev_path, path, cur, level, count, res)
        if greater(cur, length(doc))
        then return NULL
        else
            if empty(path)
            then
                // We're inside
                if prefixed("node ", doc[cur])
                    or prefixed("ref node ", doc[cur])
                then return GET_NODE(
                    doc,
                    prev_path,
                    path,
                    cur + 1,
                    count,
                    level + 1,
                    concat(res, doc[cur])
                )
                else
                    if equal("end node", doc[cur])
                        or equal("end ref node", doc[cur])
                    then
                        if equal(level, 0)
                        then return concat(res, doc[cur])
                        else return GET_NODE(
                            doc,
                            prev_path,
                            path,
                            cur + 1,
                            count,
                            level - 1,
                            concat(res, doc[cur])
                        )
                    else return GET_NODE(
                        doc,
                        prev_path,
                        path,
                        cur + 1,
                        count,
                        level,
                        concat(res, doc[cur])
                    )
            else
                if equal("node " + path[0], doc[cur])
                then return GET_NODE(
                    doc,
                    prev_path,
                    tail(path),
                    cur + 1,
                    level,
                    count,
                    concat(res, doc[cur])
                )
                else
                    if equal("ref node " + path[0], doc[cur])
                    then
                        if equal(count, 1)
                        then return GET_NODE(
                            doc,
                            prev_path,
                            tail(path),
                            cur + 1,
                            level,
                            count,
                            concat(res, doc[cur])
                        )
                        else return GET_NODE(
                            doc,
                            prev_path,
                            prev_path,
                            cur + 1,
                            level,
                            count + 1,
                            res
                        )
                    else return GET_NODE(
                        doc,
                        prev_path,
                        path,
                        cur + 1,
                        level,
                        count,
                        res
                    )
    ```
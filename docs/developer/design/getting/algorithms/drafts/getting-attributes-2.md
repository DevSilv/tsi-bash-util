```
GET_ATTR (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> attr|NULL
    if greater(length(path), 1)
    then
        if std_node_start(doc[doc_cur])
        then return HANDLE_STD_NODE_START(path, path_lvl, doc, doc_cur, doc_lvl)
        else
            if ref_node_start(doc[doc_cur])
            then return HANDLE_REF_NODE_START(
                path, path_lvl, doc, doc_cur, doc_lvl
            )
            else
                if std_node_end(doc[doc_cur])
                then return HANDLE_STD_NODE_END(
                    path, path_lvl, doc, doc_cur, doc_lvl
                )
                else
                    if ref_node_end(doc[doc_cur])
                    then return HANDLE_REF_NODE_END(
                        path, path_lvl, doc, doc_cur, doc_lvl
                    )
                    else return GET_ATTR(
                        path, path_lvl, doc, doc_cur + 1, doc_lvl
                    )
    else
        if std_attr_start(doc[doc_cur])
        then return HANDLE_STD_ATTR_START(path, path_lvl, doc, doc_cur, doc_lvl)
            if ref_attr_start(doc[doc_cur])
            then return HANDLE_REF_ATTR_START(
                path, path_lvl, doc, doc_cur, doc_lvl
            )
            else return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl)
        else return NULL

HANDLE_STD_NODE_START (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    if equal(path_lvl, doc_lvl)
        and node_contained(doc[doc_cur], path[0])
    then return GET_ATTR(
        subarr(path, 2, length(path)),
        path_lvl + 1,
        doc,
        doc_cur + 1,
        doc_lvl + 1
    )
    else return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl + 1)

HANDLE_STD_NODE_END (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl - 1)

HANDLE_REF_NODE_START (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    if equal(doc_lvl, 0)
    then return HANDLE_REF_NODE_DEFN_START(
        path, path_lvl, doc, doc_cur, doc_lvl, decl_num
    )
    else return HANDLE_REF_NODE_DECL_START(
        path, path_lvl, doc, doc_cur, doc_lvl, decl_num
    )

HANDLE_REF_NODE_DECL_START (
    path, path_lvl, doc, doc_cur, doc_lvl, decl_num
) -> ()
    if node_contained(doc[doc_cur], path[0])
    then return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num)
    else return GET_ATTR(
        path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num + 1
    )

HANDLE_REF_NODE_DEFN_START (
    path, path_lvl, doc, doc_cur, doc_lvl, decl_num
) -> ()
    if node_contained(doc[doc_cur], path[0])
    then return GET_ATTR(
        subarr(path, 2, length(path)),
        path_lvl + 1,
        doc,
        doc_cur + 1,
        doc_lvl + 1,
        decl_num
    )
    else return GET_ATTR(
        path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num - 1
    )

HANDLE_REF_NODE_END (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl - 1, decl_num)

HANDLE_STD_ATTR_START (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    if attr_contained(doc[doc_cur], path[0])
    then return GET_FULL_ATTR(doc, doc_cur + 1, [doc[doc_cur]])
    else return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num)

HANDLE_REF_ATTR_START (path, path_lvl, doc, doc_cur, doc_lvl, decl_num) -> ()
    if equal(doc_lvl, 0)
    then return HANDLE_REF_ATTR_DEFN_START(
        path, path_lvl, doc, doc_cur, doc_lvl, decl_num
    )
    else return HANDLE_REF_ATTR_DECL_START(
        path, path_lvl, doc, doc_cur, doc_lvl, decl_num
    )

HANDLE_REF_ATTR_DECL_START (
    path, path_lvl, doc, doc_cur, doc_lvl, decl_num
) -> ()
    if attr_contained(doc[doc_cur], path[0])
    then return GET_ATTR(path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num)
    else return GET_ATTR(
        path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num + 1
    )

HANDLE_REF_ATTR_DEFN_START (
    path, path_lvl, doc, doc_cur, doc_lvl, decl_num
) -> ()
    if attr_contained(doc[doc_cur], path[0])
    then return GET_FULL_ATTR(doc, doc_cur + 1, [doc[doc_cur]])
    else return GET_ATTR(
        path, path_lvl, doc, doc_cur + 1, doc_lvl, decl_num - 1
    )

GET_FULL_ATTR (doc, doc_cur, res) -> attr
    if attr_value(doc[doc_cur])
    then return GET_FULL_ATTR(doc, doc_cur, concat(res, doc_cur))
    else return res
```
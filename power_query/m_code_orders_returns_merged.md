📄 Power Query M‑Code — Merge Orders + Returns

let
    Orders = Orders_Query,
    Returns = Returns_Query,

    MergedTables = Table.NestedJoin(
        Orders,
        {"Order ID"},
        Returns,
        {"Order ID"},
        "Returns",
        JoinKind.LeftOuter
    ),

    ExpandedReturns = Table.ExpandTableColumn(MergedTables, "Returns", {"Return Flag"}, {"Return Flag"}),

    // Replace nulls with false
    CleanReturnFlag = Table.ReplaceValue(
        ExpandedReturns,
        null,
        false,
        Replacer.ReplaceValue,
        {"Return Flag"}
    )
in
    CleanReturnFlag

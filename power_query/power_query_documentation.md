📘 Power Query Documentation
 
This document explains the Power Query transformations applied to the Orders and Returns tables, including data cleaning, calculated fields, and the merge process used to create the final analysis dataset.

🛒 1. Orders Query
Purpose
Prepare the Orders dataset for analysis by:

Promoting headers

Setting correct data types

Cleaning text fields

Creating the Profit Margin calculated column

Key Transformations
Converted date fields to proper date types

Converted numeric fields (Sales, Profit, Quantity, Discount)

Trimmed text fields to remove extra spaces

Added Profit Margin column using Profit / Sales

M‑Code
let
    Source = Excel.Workbook(File.Contents("C:\Retail\orders.xlsx"), null, true),
    Orders_Sheet = Source{[Item="Orders",Kind="Sheet"]}[Data],
    PromotedHeaders = Table.PromoteHeaders(Orders_Sheet, [PromoteAllScalars=true]),
    ChangedTypes = Table.TransformColumnTypes(PromotedHeaders,{
        {"Order ID", type text},
        {"Order Date", type date},
        {"Ship Date", type date},
        {"Ship Mode", type text},
        {"Customer ID", type text},
        {"Customer Name", type text},
        {"Segment", type text},
        {"Country", type text},
        {"City", type text},
        {"State", type text},
        {"Postal Code", Int64.Type},
        {"Region", type text},
        {"Product ID", type text},
        {"Category", type text},
        {"Sub-Category", type text},
        {"Product Name", type text},
        {"Sales", type number},
        {"Quantity", Int64.Type},
        {"Discount", type number},
        {"Profit", type number}
    }),

    AddedProfitMargin = Table.AddColumn(ChangedTypes, "Profit Margin", each [Profit] / [Sales], type number),

    TrimmedText = Table.TransformColumns(AddedProfitMargin, {
        {"Customer Name", Text.Trim, type text},
        {"Product Name", Text.Trim, type text}
    })

    🔄 2. Returns Query
Purpose
Prepare the Returns dataset and create a Return Flag for easier analysis.

Key Transformations
Promoted headers

Set correct data types

Added Return Flag column (true/false)

M‑Code
let
    Source = Excel.Workbook(File.Contents("C:\Retail\returns.xlsx"), null, true),
    Returns_Sheet = Source{[Item="Returns",Kind="Sheet"]}[Data],
    PromotedHeaders = Table.PromoteHeaders(Returns_Sheet, [PromoteAllScalars=true]),
    ChangedTypes = Table.TransformColumnTypes(PromotedHeaders,{
        {"Order ID", type text},
        {"Returned", type text}
    }),

    AddedReturnFlag = Table.AddColumn(ChangedTypes, "Return Flag", each if [Returned] = "Yes" then true else false, type logical)
in
    AddedReturnFlag

🔗 3. Merge: Orders + Returns
Purpose
Combine the Orders and Returns tables to create a single dataset with return information included.

Key Transformations
Left join on Order ID

Expanded the Return Flag column

Replaced null values with false

M‑Code
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

    CleanReturnFlag = Table.ReplaceValue(
        ExpandedReturns,
        null,
        false,
        Replacer.ReplaceValue,
        {"Return Flag"}
    )
in
    CleanReturnFlag

⭐ Summary
This Power Query process ensures:

Clean, typed, analysis‑ready data

Accurate Profit Margin calculations

A unified dataset with return behavior included

Consistent formatting for downstream PivotTables and dashboards
in
    TrimmedText

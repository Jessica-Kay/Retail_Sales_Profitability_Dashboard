📄 Power Query M‑Code — Orders Table

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

    // Create Profit Margin
    AddedProfitMargin = Table.AddColumn(ChangedTypes, "Profit Margin", each [Profit] / [Sales], type number),

    // Clean text fields
    TrimmedText = Table.TransformColumns(AddedProfitMargin, {
        {"Customer Name", Text.Trim, type text},
        {"Product Name", Text.Trim, type text}
    })
in
    TrimmedText

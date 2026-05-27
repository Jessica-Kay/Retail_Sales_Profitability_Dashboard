📄 Power Query M‑Code — Returns Table

let
    Source = Excel.Workbook(File.Contents("C:\Retail\returns.xlsx"), null, true),
    Returns_Sheet = Source{[Item="Returns",Kind="Sheet"]}[Data],
    PromotedHeaders = Table.PromoteHeaders(Returns_Sheet, [PromoteAllScalars=true]),
    ChangedTypes = Table.TransformColumnTypes(PromotedHeaders,{
        {"Order ID", type text},
        {"Returned", type text}
    }),

    // Convert Returned column to true/false
    AddedReturnFlag = Table.AddColumn(ChangedTypes, "Return Flag", each if [Returned] = "Yes" then true else false, type logical)
in
    AddedReturnFlag

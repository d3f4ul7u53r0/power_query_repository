
## Dirty
---

| Order ID       | Category             | Amount                            |
| -------------- | -------------------- | --------------------------------- |
| CA-2011-167199 | Binders \| Art \| Phones \| Fasteners \| Paper | 609.98 \| 5.48 \| 391.98 \| 755.96 \| 31.12 |
| CA-2011-149020 | Office Supplies \| Furniture                   | 2.98 \| 51.94                     |
| CA-2011-131905 | Office Supplies \| Technology \| Technology    | 7.2 \| 42.0186 \| 42.035          |
| CA-2011-127614 | Accessories \| Tables \| Binders               | 234.45 \| 1256.22 \| 17.46        |

### Clean
------

| Order ID       | Category        | Amount  |
| -------------- | --------------- | ------- |
| CA-2011-167199 | Binders         | 609,98  |
| CA-2011-167199 | Art             | 5,48    |
| CA-2011-167199 | Phones          | 391,98  |
| CA-2011-167199 | Fasteners       | 755,96  |
| CA-2011-167199 | Paper           | 31,12   |
| CA-2011-149020 | Office Supplies | 2,98    |
| CA-2011-149020 | Furniture       | 51,94   |
| CA-2011-131905 | Office Supplies | 7,2     |
| CA-2011-131905 | Technology      | 42,0186 |
| CA-2011-131905 | Technology      | 42,035  |
| CA-2011-127614 | Accessories     | 234,45  |
| CA-2011-127614 | Tables          | 1256,22 |
| CA-2011-127614 | Binders         | 17,46   |

### Solution
------

```powerquery
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Order ID", type text}, {"Category", type text}, {"Amount", type text}}),
    #"Transposed Table" = Table.Transpose(#"Changed Type"),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Transposed Table", "Column1", Splitter.SplitTextByDelimiter(" | ", QuoteStyle.Csv), {"Column1.1", "Column1.2", "Column1.3", "Column1.4", "Column1.5"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Column1.1", type text}, {"Column1.2", type text}, {"Column1.3", type text}, {"Column1.4", type text}, {"Column1.5", type text}}),
    #"Split Column by Delimiter1" = Table.SplitColumn(#"Changed Type1", "Column2", Splitter.SplitTextByDelimiter(" | ", QuoteStyle.Csv), {"Column2.1", "Column2.2"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"Column2.1", type text}, {"Column2.2", type text}}),
    #"Split Column by Delimiter2" = Table.SplitColumn(#"Changed Type2", "Column3", Splitter.SplitTextByDelimiter(" | ", QuoteStyle.Csv), {"Column3.1", "Column3.2", "Column3.3"}),
    #"Changed Type3" = Table.TransformColumnTypes(#"Split Column by Delimiter2",{{"Column3.1", type text}, {"Column3.2", type text}, {"Column3.3", type text}}),
    #"Split Column by Delimiter3" = Table.SplitColumn(#"Changed Type3", "Column4", Splitter.SplitTextByDelimiter(" | ", QuoteStyle.Csv), {"Column4.1", "Column4.2", "Column4.3"}),
    #"Changed Type4" = Table.TransformColumnTypes(#"Split Column by Delimiter3",{{"Column4.1", type text}, {"Column4.2", type text}, {"Column4.3", type text}}),
    #"Transposed Table1" = Table.Transpose(#"Changed Type4"),
    #"Filled Down" = Table.FillDown(#"Transposed Table1",{"Column1"}),
    #"Renamed Columns" = Table.RenameColumns(#"Filled Down",{{"Column1", "OrderID"}, {"Column2", "Category"}, {"Column3", "Amount"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns",".",",",Replacer.ReplaceText,{"Amount"}),
    #"Changed Type5" = Table.TransformColumnTypes(#"Replaced Value",{{"Amount", type number}})
in
    #"Changed Type5"
```

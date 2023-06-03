## Dirty

Name Hussein Hakeem Address Number 22 Fioye Crescent Surulere Lagos Age 17 Gender Male
Name Arojoye Samuel Address 11 Omolade Close Omole Estate Lagos Age 16 Gender Male
Name Alex Ezurum Address 1 Adamu Lane, Abuja Age 14 Gender Male
Name Susan Nwaimo Address Number 58 Yaba Street, Kaduna State  Age 16 Gender Female
Name Ajao Opeyemi Address No12 Olubunmi Street, Abeokuta Age 18 Gender Female
Name Banjoko Adebusola Address 34 Ngige Street, Ugheli, Delta Age 14 Gender Female
Name Muhammed Olabisi Address 13, ICAN road, Enugu Age 12 Gender Female
Name Oluwagbemi Mojisola Address ACCA Lane, Onitsha Age 13 Gender Female

## Clean

| Name              | Address                               | Age | Gender |
|-------------------|---------------------------------------|-----|--------|
| Ajao Opeyemi      | No12 Olubunmi Street, Abeokuta        | 18  | Female |
| Alex Ezurum       | 1 Adamu Lane, Abuja                    | 14  | Male   |
| Arojoye Samuel    | 11 Omolade Close Omole Estate Lagos    | 16  | Male   |
| Banjoko Adebusola | 34 Ngige Street, Ugheli, Delta          | 14  | Female |
| Hussein Hakeem    | Number 22 Fioye Crescent Surulere Lagos | 17  | Male   |
| Muhammed Olabisi  | 13, ICAN road, Enugu                    | 12  | Female |
| Oluwagbemi Mojisola | ACCA Lane, Onitsha                     | 13  | Female |
| Susan Nwaimo      | Number 58 Yaba Street, Kaduna State     | 16  | Female |

## Solution

```powerquery
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Column1", type text}}),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Changed Type", "Column1", Splitter.SplitTextByEachDelimiter({" "}, QuoteStyle.Csv, false), {"Column1.1", "Column1.2"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Column1.1", type text}, {"Column1.2", type text}}),
    #"Inserted Text Before Delimiter" = Table.AddColumn(#"Changed Type1", "Text Before Delimiter", each Text.BeforeDelimiter([Column1.2], " ", 1), type text),
    #"Reordered Columns" = Table.ReorderColumns(#"Inserted Text Before Delimiter",{"Column1.1", "Text Before Delimiter", "Column1.2"}),
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Reordered Columns", "Text Between Delimiters", each Text.BetweenDelimiters([Column1.2], " ", " ", 1, 0), type text),
    #"Reordered Columns1" = Table.ReorderColumns(#"Inserted Text Between Delimiters",{"Column1.1", "Text Before Delimiter", "Text Between Delimiters", "Column1.2"}),
    #"Inserted Text Between Delimiters1" = Table.AddColumn(#"Reordered Columns1", "Text Between Delimiters.1", each Text.BetweenDelimiters([Column1.2], " ", "Age", 2, 0), type text),
    #"Reordered Columns2" = Table.ReorderColumns(#"Inserted Text Between Delimiters1",{"Column1.1", "Text Before Delimiter", "Text Between Delimiters", "Text Between Delimiters.1", "Column1.2"}),
    #"Extracted Text After Delimiter" = Table.TransformColumns(#"Reordered Columns2", {{"Column1.2", each Text.AfterDelimiter(_, " ", {3, RelativePosition.FromEnd}), type text}}),
    #"Split Column by Delimiter1" = Table.SplitColumn(#"Extracted Text After Delimiter", "Column1.2", Splitter.SplitTextByDelimiter(" ", QuoteStyle.Csv), {"Column1.2.1", "Column1.2.2", "Column1.2.3", "Column1.2.4"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"Column1.2.1", type text}, {"Column1.2.2", Int64.Type}, {"Column1.2.3", type text}, {"Column1.2.4", type text}}),
    #"Pivoted Column" = Table.Pivot(#"Changed Type2", List.Distinct(#"Changed Type2"[Column1.1]), "Column1.1", "Text Before Delimiter"),
    #"Pivoted Column1" = Table.Pivot(#"Pivoted Column", List.Distinct(#"Pivoted Column"[#"Text Between Delimiters"]), "Text Between Delimiters", "Text Between Delimiters.1"),
    #"Pivoted Column2" = Table.Pivot(#"Pivoted Column1", List.Distinct(#"Pivoted Column1"[Column1.2.1]), "Column1.2.1", "Column1.2.2"),
    #"Pivoted Column3" = Table.Pivot(#"Pivoted Column2", List.Distinct(#"Pivoted Column2"[Column1.2.3]), "Column1.2.3", "Column1.2.4")
in
    #"Pivoted Column3"
```
## Dirty

| Description                       | Quantity |
|-----------------------------------|----------|
| 10% Dextrose Water 500ml          | 9Pcs     |
| syr Cough Syrup (P) 100ml         | 9Bottle  |
| Susp. Amoxicillin 125mg/5ml       | 9Bottle  |
| Susp. Amoxicillin 125mg/5ml       | 9Bottle  |
| susp Metronidazole 100mg/5ml      | 9Bottle  |
| susp Magnessium Trisilicate 200ml | 9Bottle  |
| inj Benzylpenicillin 1mu          | 98Vial   |
| inj Benzylpenicillin 1mu          | 96Vial   |
| inj Gentamicin 80mg               | 94Amp    |
| inj Benzylpenicillin 1mu          | 90Vial   |
| Syrup Paracetamol 125mg/5ml       | 90Bottle |
| inj Hydrocortisone 100mg          | 8Vial    |
| inj Hydrocortisone 100mg          | 8Vial    |
| Gutt/Ear Gentamycin 0.05          | 8Tube    |
| Chlorhexidine Gel                 | 8Tube    |
| Blood Giving set                  | 8Pcs     |
| syr Vitamin B complex             | 8Bottle  |
| Syr Paracetamol Drop              | 8Bottle  |
| syr Cough Syrup (P) 100ml         | 8Bottle  |
| Syr Albendazole 100mg/5ml         | 8Bottle  |
| Syr Albendazole 100mg/5ml         | 8Bottle  |
| Syr Albendazole 100mg/5ml         | 8Bottle  |
| Syr Albendazole 100mg/5ml         | 8Bottle  |
| Syr Albendazole 100mg/5ml         | 8Bottle  |

(...)

## Clean

| Description                    | Quantity | Measure |
|--------------------------------|----------|---------|
| 5% Dextrose saline 1000ml      | 100      | Pcs     |
| 4.3% Dextrose Saline 500ml     | 87       | Pcs     |
| 5% Dextrose saline 500ml       | 73       | Pcs     |
| 5% Dextrose saline 1000ml      | 50       | Pcs     |
| 5% Dextrose water 500ml        | 50       | Pcs     |
| 5% Dextrose saline 1000ml      | 44       | Pcs     |
| 5% Dextrose water 500ml        | 40       | Pcs     |
| 5% Dextrose water 500ml        | 32       | Pcs     |
| 5% Dextrose water 500ml        | 30       | Pcs     |
| 5% Dextrose saline 1000ml      | 29       | Pcs     |
| 10% Mannitol                   | 26       | Pcs     |
| 5% Dextrose saline 500mls      | 26       | Pcs     |
| 5% Dextrose saline 1000ml      | 20       | Pcs     |
| 5% Dextrose saline 1000ml      | 20       | Pcs     |
| 5% Dextrose saline 1000ml      | 20       | Pcs     |
| 5% Dextrose water 1000ml       | 13       | Pcs     |
| 5% Dextrose saline 1000ml      | 10       | Pcs     |
| 5% Dextrose water 500ml        | 10       | Pcs     |
| 5% Dextrose saline 500mls      | 6        | Pcs     |
| 5% Dextrose saline 1000ml      | 0        | Pcs     |
| 5% Dextrose water 500ml        | 0        | Pcs     |
| 5% Dextrose saline 1000ml      | 0        | Pcs     |
| 5% Dextrose water 500ml        | 0        | Pcs     |
| 5% Dextrose saline 1000ml      | 0        | Pcs     |

(...)

## Solution

```powerquery
let
    Source = Excel.CurrentWorkbook(){[Name="Table1"]}[Content],
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Description", type text}, {"Quantity", type text}}),
    #"Replaced Value" = Table.ReplaceValue(#"Changed Type",".",",",Replacer.ReplaceText,{"Quantity"}),
    #"Split Column by Character Transition" = Table.SplitColumn(#"Replaced Value", "Quantity", Splitter.SplitTextByCharacterTransition({"0".."9",","}, (c) => not List.Contains({"0".."9",","}, c)), {"Quantity.1", "Quantity.2"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Character Transition",{{"Quantity.1", type number}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type1",{{"Quantity.1", "Quantity"}, {"Quantity.2", "Measure"}})
in
    #"Renamed Columns"
```
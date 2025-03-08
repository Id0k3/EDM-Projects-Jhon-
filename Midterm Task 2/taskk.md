let
    Source = Csv.Document(File.Contents("C:\Users\COMLAB\Downloads\Uncleaned_DS_jobs.csv"),[Delimiter=",", Columns=15, Encoding=1252, QuoteStyle=QuoteStyle.Csv]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"index", Int64.Type}, {"Job Title", type text}, {"Salary Estimate", type text}, {"Job Description", type text}, {"Rating", type number}, {"Company Name", type text}, {"Location", type text}, {"Headquarters", type text}, {"Size", type text}, {"Founded", Int64.Type}, {"Type of ownership", type text}, {"Industry", type text}, {"Sector", type text}, {"Revenue", type text}, {"Competitors", type text}}),
    #"Extracted Text Before Delimiter" = Table.TransformColumns(#"Changed Type", {{"Salary Estimate", each Text.BeforeDelimiter(_, "("), type text}}),
    #"Inserted Literal" = Table.AddColumn(#"Extracted Text Before Delimiter", "Min Sal", each "101", type text),
    #"Inserted Literal1" = Table.AddColumn(#"Inserted Literal", "Max Sal", each "101", type text),
    #"Added Custom" = Table.AddColumn(#"Inserted Literal1", "Role Type", each if Text.Contains([Job Title], "Data Scientist") then
"Data Scientist"
else if Text.Contains([Job Title], "Data Analyst") then
"Data Analyst"
else if Text.Contains([Job Title], "Data Engineer") then
"Data Engineer"
else if Text.Contains([Job Title], "Machine Learning") then
"Machine Learning Engineer"
else
"other"),
    #"Changed Type1" = Table.TransformColumnTypes(#"Added Custom",{{"Role Type", type text}}),
    #"Added Custom1" = Table.AddColumn(#"Changed Type1", "Location Correction", each if [Location]= "New Jersey" then ", NJ"
else if [Location] = "Remote" then ", other"
else if [Location]= "United States" then ", other"
else if [Location]= "Texas" then ", TX"
else if [Location]= "Patuxent" then ", MA"
else if [Location]= "California" then ", CA"
else if [Location]= "Utah" then ", UT"
else [Location]),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Added Custom1", "Location Correction", Splitter.SplitTextByDelimiter(",", QuoteStyle.Csv), {"Location Correction.1", "Location Correction.2"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Location Correction.1", type text}, {"Location Correction.2", type text}}),
    #"Replaced Value" = Table.ReplaceValue(#"Changed Type2","Anne Arundel","MA",Replacer.ReplaceText,{"Location Correction.2"}),
    #"Renamed Columns" = Table.RenameColumns(#"Replaced Value",{{"Location Correction.2", "State Abbreviations"}}),
    #"Inserted Text Before Delimiter" = Table.AddColumn(#"Renamed Columns", "MinCompanySize", each Text.BeforeDelimiter([Size], " "), type text),
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Inserted Text Before Delimiter", "MaxCompanySize", each Text.BetweenDelimiters([Size], " ", " ", 1, 0), type text),
    #"Filtered Rows" = Table.SelectRows(#"Inserted Text Between Delimiters", each ([Competitors] <> "-1") and ([Revenue] <> "Unknown / Non-Applicable") and ([Industry] <> "-1")),
    #"Split Column by Delimiter1" = Table.SplitColumn(#"Filtered Rows", "Company Name", Splitter.SplitTextByDelimiter("#(lf)", QuoteStyle.Csv), {"Company Name.1", "Company Name.2"}),
    #"Changed Type3" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"Company Name.1", type text}, {"Company Name.2", type number}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type3",{"Company Name.2", "Job Description"})
in
    #"Removed Columns"

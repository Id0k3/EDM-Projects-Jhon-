# Midterm Lab Task 2 - Data Cleaning and Transformation using POWER QUERY
let
    Source = Csv.Document(File.Contents("C:\Users\PC\Downloads\Uncleaned_DS_jobs.csv"),[Delimiter=",", Columns=15, Encoding=65001, QuoteStyle=QuoteStyle.Csv]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"index", Int64.Type}, {"Job Title", type text}, {"Salary Estimate", type text}, {"Job Description", type text}, {"Rating", type number}, {"Company Name", type text}, {"Location", type text}, {"Headquarters", type text}, {"Size", type text}, {"Founded", Int64.Type}, {"Type of ownership", type text}, {"Industry", type text}, {"Sector", type text}, {"Revenue", type text}, {"Competitors", type text}}),
    #"Extracted Text Before Delimiter" = Table.TransformColumns(#"Changed Type", {{"Salary Estimate", each Text.BeforeDelimiter(_, "("), type text}}),
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Extracted Text Before Delimiter", "Min Sal", each Text.BetweenDelimiters([Salary Estimate], "$", "K"), type text),
    #"Inserted Text Between Delimiters1" = Table.AddColumn(#"Inserted Text Between Delimiters", "Max Sal", each Text.BetweenDelimiters([Salary Estimate], "$", "K", 1, 0), type text),
    #"Added Custom" = Table.AddColumn(#"Inserted Text Between Delimiters1", "Role Type", each if Text.Contains([Job Title], "Data Scientist") then  
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
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type2",{{"Location Correction.2", "State Abbreviations"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns","Anne Arundel","MA",Replacer.ReplaceText,{"State Abbreviations"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","employees","",Replacer.ReplaceText,{"Size"}),
    #"Inserted Text Before Delimiter" = Table.AddColumn(#"Replaced Value1", "MinCompanySize", each Text.BeforeDelimiter([Size], " "), type text),
    #"Inserted Text Between Delimiters2" = Table.AddColumn(#"Inserted Text Before Delimiter", "MaxCompanySize", each Text.BetweenDelimiters([Size], " ", " ", 1, 0), type text),
    #"Replaced Value2" = Table.ReplaceValue(#"Inserted Text Between Delimiters2","-1","N/A",Replacer.ReplaceText,{"Competitors"}),
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","-1","0",Replacer.ReplaceText,{"Revenue"}),
    #"Replaced Value4" = Table.ReplaceValue(#"Replaced Value3","-1","Other",Replacer.ReplaceText,{"Industry"}),
    #"Changed Type3" = Table.TransformColumnTypes(#"Replaced Value4",{{"Company Name", type text}}),
    #"Split Column by Delimiter1" = Table.SplitColumn(#"Changed Type3", "Company Name", Splitter.SplitTextByDelimiter("#(lf)", QuoteStyle.Csv), {"Company Name.1", "Company Name.2"}),
    #"Changed Type4" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"Company Name.1", type text}, {"Company Name.2", type number}}),
    #"Renamed Columns1" = Table.RenameColumns(#"Changed Type4",{{"Company Name.1", "Company Name"}}),
    #"Removed Columns" = Table.RemoveColumns(#"Renamed Columns1",{"Company Name.2", "Job Description"})
in
    #"Removed Columns"

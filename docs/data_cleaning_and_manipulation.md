## Process

### Data
Based on the business task, i chose to work on select files:
 -dailyActivity_merged
- sleepDay_merged
- heartrate_seconds_merged
- weightLogInfo_merged
- dailyCalories_merged
- dailyIntensities_merged
- minuteSleep_merged
- minuteMETsNarrow_merged
- hourlySteps_merged

### Data Importing
for Data processing i chose to use Excel as the main tool. So i had to import all the files i need, but they were too many and
it would require too much repetitive work. So i searched and learned about VBA Macros. With the help of ChatGPT I generated a script that i would run to load every file into a separate sheet in the workbook. Here is the code i ran:

```
Sub ImportCSVIntoSeparateSheets()
    Dim folderPath As String
    Dim csvFile As String
    Dim ws As Worksheet
    Dim wb As Workbook
    
    ' Update the folder path to the location of your CSV files
    folderPath = "C:\Users\user\Desktop\archive\mturkfitbit_export_4.12.16-5.12.16\Fitabase Data 4.12.16-5.12.16\"
    
    ' Get the first CSV file from the folder
    csvFile = Dir(folderPath & "*.csv")
    
    ' Loop through all CSV files in the folder
    Do While csvFile <> ""
        ' Open CSV file and add it as a new sheet
        Set wb = Workbooks.Open(folderPath & csvFile)
        wb.Sheets(1).Copy After:=ThisWorkbook.Sheets(ThisWorkbook.Sheets.Count)
        
        ' Rename the sheet to the CSV file name (without the extension)
        ActiveSheet.Name = Left(csvFile, InStrRev(csvFile, ".") - 1)
        
        ' Close the original CSV file without saving
        wb.Close False
        
        ' Get the next CSV file
        csvFile = Dir
    Loop
End Sub


```


### Data Integrity Check
- Loaded the dataset into [Tool Used] and performed an initial inspection for:
  - Missing values
  - Duplicates
  - Inconsistent data types

### Chosen Tools
- **[Tool Name]**: Selected for its ability to [reason for choice, e.g., perform data transformations efficiently, user-friendly interface, etc.].

### Data Transformation Steps
1. **Removed Duplicates**: Identified and removed any duplicate entries.
2. **Handled Missing Values**: 
   - Imputed missing values in [Column Name] using [method used, e.g., mean, median, mode].
   - Removed rows with excessive missing values.
3. **Standardized Data Types**: Ensured all columns had the correct data types (e.g., converting date strings to date format).
4. **Renamed Columns**: Updated column names for clarity (e.g., `calories_burned` to `Calories Burned`).
5. **Created New Columns**: Added [any new columns created] for enhanced analysis.

### Documentation of Cleaning Process
- All steps taken during the cleaning process have been recorded for review and sharing.
- Future analyses will rely on this clean dataset to ensure accurate insights.

---

**Next Steps:**  
Proceed to the analysis phase to uncover insights from the cleaned data.


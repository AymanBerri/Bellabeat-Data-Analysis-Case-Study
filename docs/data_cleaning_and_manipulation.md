## Process

### Data Selection
Based on the business task, I selected specific files that provide relevant data for the analysis:

<details>
  <summary>Click to view selected CSV files</summary>

  - `dailyActivity_merged`
  - `sleepDay_merged`
  - `heartrate_seconds_merged`
  - `weightLogInfo_merged`
  - `dailyCalories_merged`
  - `dailyIntensities_merged`
  - `minuteSleep_merged`
  - `minuteMETsNarrow_merged`
  - `hourlySteps_merged`
  - `dailySteps_merged`

</details>

### Data Importing
For data processing, I chose to use **Excel** as the main tool. Since manually importing each CSV file would be too repetitive, I utilized **VBA Macros** to automate the process. This allowed me to load each CSV file into a separate sheet in my workbook.

#### VBA Macro Code
Here is the code I ran to automatically import all the necessary CSV files:

```vba
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


### Data Cleaning Steps

| Step                         | Description                                                                 |
|------------------------------|-----------------------------------------------------------------------------|
| **Checked for Missing Values** | I identified columns with missing data and decided to fill them with the mean where appropriate. |
| **Checked for Duplicates**    | I searched for any duplicate entries in the dataset and removed them to ensure data integrity. |
| **Corrected Data Types**      | I ensured that all columns were in the correct data type, converting any necessary columns to the appropriate format. |
| **Standardized Values**       | I standardized categorical values to ensure consistency (e.g., "yes," "Yes," and "YES" were all converted to "Yes"). |
| **Identified Outliers**       | I used statistical methods to identify outliers and decided to cap extreme values to maintain data quality. |
| **Documented Changes**        | I kept a record of all changes made during the cleaning process for future reference. |






1) ## Check for missing values
  started by filtering and sorting data to check for null or blank fields, found none. With the help of ChatGPT, I generated a script that  checks for blank fields in the entire workbook so this is the script i ran:
  ```vba
  Sub CheckForBlanks()
      Dim ws As Worksheet
      Dim cell As Range
      Dim blankCount As Long
      Dim report As String
  
      report = "Blank Cells Report:" & vbCrLf
  
      For Each ws In ThisWorkbook.Worksheets
          blankCount = 0
          For Each cell In ws.UsedRange
              If IsEmpty(cell) Then
                  blankCount = blankCount + 1
              End If
          Next cell
          report = report & ws.Name & ": " & blankCount & " blank cells" & vbCrLf
      Next ws
  
      MsgBox report
  End Sub
  
  ```
  Result:
  ![image](https://github.com/user-attachments/assets/7041d604-c80e-4eda-ad16-2d7091f7cb43)
  
  In the `weightLogInfo_merged` file, there is a column "Fat" that has only 2 values out of 65 values. The column was deleted.

2) ## Check for duplicates
  using excels Remove Duplicates tool, in `minutesleep_merged` 543 duplicates were removed and 187978 remained while in `sleepday_merged` 3 duplicates were removed and 410 unique values remained.

3) ## Trimming the data
   Now i decided to trim the data to remove any leading or trailing whitespaces. and the way i do this is by using Excel's Find and Replace tool. But im faced with a problem here where in some sheets there is data that is seperated by white spaces instead of each having its own column. So what i did for Date columns that had the date and time in the same cell is split them by simply creating a new column by extracting the time from the Date column ```=TIME(HOUR(B2), MINUTE(B2), SECOND(B2))```, then using `=INT(A1)` to extract the date into a new column. After deleting the original column, i search again for whitespaces using the Find and Replace tool to check if the data is trimmed. Altered sheets:
     - `heartrate_seconds_merged` i split the column `Time` to `Date` and `Time`.
     - `hourlySteps_merged` i split the `ActivityHour` to `ActivityDate` for the date and `ActivityHour` for the hour.
     - `minuteMETsNarrow_merged` i split `ActivityMinute` to `ActivityDate` and `ActivityMinute`.
     - `minuteSleep_merged` i split `date` to `Date` and `Time`.
     - `sleepDay_merged` i split `SleepDay` to `SleepDay` and `SleepTime`
     - `weightLogInfo_merged` i split `Date` to `Date` and `Time`.


3) ## Corrected Data Types
   now comes where i ensure all columns are the correct data type and convert and necessary columns to the appropriate format.
   All ID columns are formatted as `Number`
   All Date columns are formatted as `Date`
   All Time columns are formatted as `Time`
   




### Documentation of Cleaning Process
- All steps taken during the cleaning process have been recorded for review and sharing.
- Future analyses will rely on this clean dataset to ensure accurate insights.

---

**Next Steps:**  
Proceed to the analysis phase to uncover insights from the cleaned data.


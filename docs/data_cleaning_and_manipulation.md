# Data Cleaning Process Summary


## Overview
This document outlines the steps taken during the data cleaning process for the dataset. Each section provides details of the methods used and results obtained.



## Table of Contents
- [Data Selection](#data-selection) 
- [Data Importing](#data-importing)
- [Check for Missing Values](#1-check-for-missing-values)
- [Check for Duplicates](#2-check-for-duplicates)
- [Trimming the Data](#3-trimming-the-data)
- [Corrected Data Types](#4-corrected-data-types)
- [Check for Outliers](#5-check-for-outliers)

---

## Data Selection
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

---

## Data Importing
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


---

### 1. Check for Missing Values
Started by filtering and sorting data to check for null or blank fields, found none. With the help of ChatGPT, I generated a script that checks for blank fields in the entire workbook.

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

**Result:**
![image](https://github.com/user-attachments/assets/7041d604-c80e-4eda-ad16-2d7091f7cb43)

In the `weightLogInfo_merged` file, there was a column "Fat" that had only 2 values out of 65. The column was deleted.



---

### 2. Check for Duplicates
Using Excel's Remove Duplicates tool:
- In `minutesleep_merged`, 543 duplicates were removed, leaving 187,978 records.
- In `sleepday_merged`, 3 duplicates were removed, resulting in 410 unique values.


---

### 3. Trimming the Data
I decided to trim the data to remove any leading or trailing whitespaces using Excel's Find and Replace tool. However, some sheets had data separated by white spaces instead of in their own columns. To address this:

- For Date columns with both date and time in the same cell:
  - Created a new column to extract the time.
  - Extracted the date into a new column.

After deleting the original column, I searched again for whitespaces to ensure data was trimmed. Altered sheets include:
- `heartrate_seconds_merged`: Split `Time` into `Date` and `Time`.
- `hourlySteps_merged`: Split `ActivityHour` into `ActivityDate` and `ActivityHour`.
- `minuteMETsNarrow_merged`: Split `ActivityMinute` into `ActivityDate` and `ActivityMinute`.
- `minuteSleep_merged`: Split `date` into `Date` and `Time`.
- `sleepDay_merged`: Split `SleepDay` into `SleepDay` and `SleepTime`.
- `weightLogInfo_merged`: Split `Date` into `Date` and `Time`.


---

### 4. Corrected Data Types
Ensured all columns are formatted correctly:
- All ID columns are formatted as `Number`.
- All Date columns are formatted as `Date`.
- All Time columns are formatted as `Time`.


---

### 5. Check for Outliers
To check for outliers, I reviewed the data by selecting columns and visualizing them using box plots or sorting them from largest to smallest to locate extreme values. Notable outliers included:
- 36,019 total steps
- 4,100 calories
- 294 pounds

I decided to keep these values as outliers can be informative and relevant in health data.



---

**Next Steps:**  
Proceed to the analysis phase to uncover insights from the cleaned data.


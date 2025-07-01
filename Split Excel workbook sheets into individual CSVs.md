## This code splits each worksheet in an excel workbook into individual csv files

```python
import pandas as pd
import os

# Path to your Excel file
excel_file = 'C:/Users/-Course-Dataset.xlsx'

# Create output folder for CSVs
output_folder = 'csv_sheets'
os.makedirs(output_folder, exist_ok=True)

# Load the Excel file
xls = pd.ExcelFile(excel_file, engine='openpyxl')

# Iterate over each sheet and export as CSV
for sheet_name in xls.sheet_names:

    #Loads each worksheet into a DataFrame
    df = pd.read_excel(xls, sheet_name=sheet_name)

    #Builds the file path for the output CSV using the sheet name
    csv_file = os.path.join(output_folder, f"{sheet_name}.csv")

    #Saves the DataFrame to a .csv file without row indices 
    df.to_csv(csv_file, index=False)

    print(f"Saved '{sheet_name}' to {csv_file}")


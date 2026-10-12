# MP1 - DS3500 - Jeremy Siu

## Explanation

This pipeline loads a CSV file and then validates and cleans it using settings from the YAML config file. After that, it outputs the cleaned CSV file under output/. The data moves through pipeline.py which uses command line arguments and several modules that live under src/ to carry out the validation and cleaning. data_loaders.py is the module loads CSV, JSON, and YAML files. utils.py is the module that sets up logging and verifies that the input files exist. data_validator.py is the module that validates the data by making sure the required columns exist and removes the invalid ones. data_processor.py is the module that cleans the data by removing duplicate rows, rows that are missing values, and rows with outliers, creating a cleaning report after that. data_output.py is the module that saves the cleaned data as a new CSV file. 

## Example

Command: python pipeline.py --input fixtures/sample_data.csv --output output/clean.csv --config config/config.yaml --verbose 

Output: 
19:07:04 DEBUG    __main__ — Arguments parsed: Namespace(input='fixtures/sample_data.csv', config='config/config.yaml', output='output/clean.csv', verbose=True)
19:07:04 INFO     src.utils — Input file validated: fixtures/sample_data.csv
19:07:04 INFO     src.utils — Input file validated: config/config.yaml
19:07:04 INFO     src.data_loaders — Loaded CSV file: fixtures/sample_data.csv (100 rows)
19:07:04 INFO     src.data_loaders — Loaded YAML file: config/config.yaml
19:07:04 WARNING  src.data_validator — Invalid numeric value in rating at row 94: 'not_available'
19:07:04 WARNING  src.data_validator — Invalid numeric value in rating at row 95: 'error'
19:07:04 WARNING  src.data_validator — Removed 2 rows with invalid numeric values in rating
19:07:04 DEBUG    src.data_validator — Validation: 100 -> 98 rows
19:07:04 INFO     __main__ — Validation complete: 100 -> 98 rows
19:07:04 DEBUG    src.data_processor — remove_duplicates: 98 -> 96 rows
19:07:04 DEBUG    src.data_processor — handle_missing: 96 -> 94 rows
19:07:04 DEBUG    src.data_processor — rating: method=iqr, threshold=1.5, removed=2
19:07:04 INFO     __main__ — Processing complete: 98 -> 92 rows
19:07:04 DEBUG    src.data_output — Saved 92 rows to output/clean.csv
19:07:04 INFO     __main__ — Saved cleaned data to output/clean.csv

Cleaning report:
{'rows_before': 98, 'rows_after': 92, 'rows_removed': 6, 'columns_before': 5, 'columns_after': 5, 'columns_removed': 0}
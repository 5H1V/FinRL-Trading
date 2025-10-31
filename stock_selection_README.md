# Stock Selection Script (stock_selection.py)

## Overview

`stock_selection.py` is a version of the `stock_selection.ipynb` Jupyter notebook converted into a command-line script. This script can batch run stock selection models for all industries and generate comprehensive stock selection results.

## Features

- **Batch Processing**: Automatically processes all 11 industries (sector10, sector15, ..., sector60)
- **Flexible Paths**: Supports custom input and output directories
- **Error Handling**: Includes file existence checks and exception handling
- **Progress Display**: Detailed processing progress and statistics
- **Automatic Directory Creation**: Automatically creates the output directory if it does not exist.

## Usage

### Basic Usage

```bash
# Use the default output directory
python stock_selection.py --data_path "./data_processor/my_outputs"

# Specify the output directory
python stock_selection.py --data_path "./data_processor/my_outputs" --output_path "./result"

# View help information
python stock_selection.py --help
```

### Parameter Description

| Parameter | Type | Required | Default Value | Description |
|------|------|------|--------|------|
| `--data_path` | str | Yes | - | Directory path for input sector files |
| `--output_path` | str | No | `./result` | Output directory path |


### Example Usage

```bash
# Example 1: Use the my_outputs directory to output to the result directory
python stock_selection.py --data_path "./data_processor/my_outputs" --output_path "./result"

# Example 2: Using a different data directory
python stock_selection.py --data_path "./data_processor/outputs" --output_path "./my_results"

# Example 3: Using an absolute path
python stock_selection.py --data_path "D:/data/sectors" --output_path "D:/results"
```

## Input File Requirements

### Required Files

1. **final_ratios.csv**: Fundamental data file
   - Location: `{data_path}/final_ratios.csv`
   - Contains fundamental ratio data for all stocks

2. **sector*.xlsx**: Industry data file
   - Location: `{data_path}/sector10.xlsx`, `{data_path}/sector15.xlsx`, ..., `{data_path}/sector60.xlsx`
   - Each file contains stock data for the corresponding industry

### File Structure Example

```
data_processor/my_outputs/
├── final_ratios.csv
├── sector10.xlsx
├── sector15.xlsx
├── sector20.xlsx
├── ...
└── sector60.xlsx
```

## Output Files

### Main Output

- **stock_selected.csv**: Stock selection results file
  - Location: `{output_path}/stock_selected.csv`
  - Contains stock selection results for all industries

### Output File Format

| Column Name | Type | Description |
|------|------|------|
| `gvkey` | int | Unique Stock Identifier |
| `predicted_return` | float | Predicted Return Rate |
| `trade_date` | str | Transaction Date |

### Intermediate Files

During script execution, model results for each industry will be generated in the `results/` directory:
```
results/
├── sector10/
│   └── df_predict_best.csv
├── sector15/
│   └── df_predict_best.csv
├── ...
└── sector60/
    └── df_predict_best.csv
```

## Processing Flow

1. **Parameter Validation**: Check input parameters and file existence.
2. **Directory Creation**: Automatically create the output directory if it does not exist.
3. **Industry Looping**: For each industry (10, 15, 20, ..., 60), perform the following:
   - Check if the sector file exists.
   - Run `fundamental_run_model.py` to train the model.
   - Read the prediction results file.
   - Select the top 25% of stocks (quantile 0.75).
   - Collect the stock selection results.
4. **Result Summarization**: Merge the results from all industries.
5. **Save Output**: Save the output to the specified output directory.

## Error Handling

The script includes the following error handling mechanisms:

- **File Not Found**: If the input file does not exist, an error message will be displayed and the script will exit.
- **Model Training Failure**: If model training fails for a particular industry, that industry will be skipped and processing will continue with other industries.
- **Missing Result File**: If the prediction result file does not exist, that industry will be skipped.
- **Data Processing Anomaly**: If an anomaly occurs while processing data for a particular industry, the error will be logged and processing will continue.

## Dependencies

- `pandas`: Data processing
- `argparse`: Command-line argument parsing
- `pathlib`: Path handling
- `os`: Operating system interface
- `time`: Time calculation
- `sys`: System-related functions

## Notes

1. **Python Version**: Python 3.7 or higher is recommended.
2. **Dependency Script**: Ensure `fundamental_run_model.py` is in the same directory.
3. **Memory Usage**: Processing large amounts of data may require significant memory.
4. **Running Time**: Running all industries completely may take a long time.
5. **File Permissions**: Ensure sufficient permissions to create output directories and files.

## Comparison with Jupyter Notebook

| Features | Jupyter Notebook | Python Scripts |
|------|------------------|------------|
| Interactivity | High | Low |
| Automation | Low | High |
| Parameterization | Hard-coded | Command-line Arguments |
| Error Handling | Manual | Automatic |
| Batch Execution | Difficult | Easy |
| Deployment | Complex | Simple |

## Troubleshooting

### Common Problems

1. **"Fundamental data file does not exist"**
   - Check if the `--data_path` parameter is correct.
   - Confirm that the `final_ratios.csv` file exists.
2. **"Sector file does not exist"**
   - Check if the sector*.xlsx file is in the specified directory.
   - Confirm that the filename format is correct.
3. **"Model training failed"**
   - Check if `fundamental_run_model.py` exists.
   - Confirm that the input data format is correct.
4. **"Prediction result file does not exist"**
   - Check if model training completed successfully.
   - Confirm that the `results/` directory structure is correct.

### Debugging Suggestions

- Use `--help` to view parameter descriptions.
- Check the existence and format of the input files.
- View detailed error messages.
- Confirm that all dependent libraries are installed.

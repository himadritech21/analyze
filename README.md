# Data Processor Project

This repository hosts a robust data processing pipeline designed to extract, transform, and load data from an Excel spreadsheet into a structured JSON output. It leverages Python with Pandas for data manipulation and GitHub Actions for continuous integration and deployment.

## Project Structure

```
.
├── .github/                       # GitHub Actions workflows
│   └── workflows/
│       └── ci.yml                 # CI/CD pipeline for linting, execution, and publishing
├── data.xlsx                      # Original input data in Excel format
├── data.csv                       # Converted input data (from data.xlsx)
├── execute.py                     # Python script for data processing
├── index.html                     # Single-file responsive HTML app (hosted via GitHub Pages)
└── LICENSE                        # MIT License
```

## Features

*   **Data Ingestion:** Reads data from `data.xlsx` (and `data.csv`).
*   **Robust Processing:** `execute.py` handles potential data quality issues, ensuring reliable numeric calculations and aggregations.
*   **JSON Output:** Processes data and generates `result.json` with aggregated insights.
*   **Continuous Integration (CI):**
    *   Code linting with `ruff` to maintain code quality.
    *   Automated execution of `execute.py`.
*   **Continuous Deployment (CD):**
    *   Publishes the generated `result.json` to GitHub Pages.
    *   `index.html` provides a user-friendly interface to navigate the project and access the latest `result.json`.

## Getting Started

### Prerequisites

*   Python 3.11+
*   pandas 2.3+
*   openpyxl (for reading .xlsx files)
*   ruff (for linting)

You can install the necessary Python packages using pip:

```bash
pip install pandas openpyxl ruff
```

### Local Execution

1.  **Clone the repository:**
    ```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
    cd YOUR_REPO_NAME
    ```
2.  **Ensure `data.xlsx` (and `data.csv`) are present:**
    The `data.xlsx` file should be in the root directory. If `data.csv` is not present, you can generate it from `data.xlsx` using a simple Pandas script (or it's included in the initial commit as per instructions).

    To generate `data.csv` from `data.xlsx` (if needed):
    ```python
import pandas as pd
pd.read_excel('data.xlsx').to_csv('data.csv', index=False)
    ```
3.  **Run the processing script:**
    ```bash
python execute.py > result.json
    ```
    This will execute the `execute.py` script, which reads `data.xlsx`, performs the necessary calculations, and prints the resulting JSON to standard output. The `> result.json` redirects this output to a file named `result.json`.

### Understanding `execute.py`

The `execute.py` script performs the following steps:
1.  Reads `data.xlsx` into a Pandas DataFrame.
2.  Robustly converts 'Quantity' and 'Price' columns to numeric types, handling non-numeric entries by coercing them to `NaN` and then filling `NaN`s with 0.
3.  Ensures a 'Category' column exists, defaulting to 'Uncategorized' if missing.
4.  Calculates `Calculated_Value` as the product of 'Quantity' and 'Price'.
5.  Groups the data by 'Category' and sums the `Calculated_Value` for each category.
6.  Outputs the aggregated results as a JSON string.

## GitHub Actions Workflow

The `.github/workflows/ci.yml` file defines a GitHub Actions workflow that runs on every `push` to the repository.

### Workflow Steps:

1.  **Checkout Code:** Clones the repository.
2.  **Set up Python:** Configures Python 3.11.
3.  **Install Dependencies:** Installs `pandas`, `openpyxl`, and `ruff`.
4.  **Run Ruff:** Executes `ruff check .` to lint the Python code and report issues in the CI log.
5.  **Generate `result.json`:** Runs `python execute.py > result.json` to create the output file.
6.  **Upload Artifact:** Uploads `result.json` as a build artifact, making it available for later steps or download.
7.  **Deploy to GitHub Pages:** Uses `actions/upload-pages-artifact` and `actions/deploy-pages` to publish the `result.json` to GitHub Pages.

### Accessing CI-Generated `result.json`

After a successful workflow run, the `result.json` will be accessible via GitHub Pages. The URL will typically be:

`https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/result.json`

(Replace `YOUR_USERNAME` and `YOUR_REPO_NAME` with your actual GitHub username and repository name.)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
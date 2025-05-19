# Video Game Sales Data Cleaning in R

This project demonstrates the data cleaning process for the `vgsales.csv` dataset, sourced from Kaggle, using R in a Jupyter Notebook. The dataset contains 16,598 records of video game sales across 11 columns, including game rank, name, platform, year, genre, publisher, and sales figures for various regions. The goal is to clean the dataset by handling missing values, duplicates, and data types, producing a cleaned file (`vgsales_cleaned.csv`) suitable for downstream analysis or visualization (e.g., in Tableau).

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Data](#data)
- [Methodology](#methodology)
- [Output](#output)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Installation
To run this project, ensure you have R and Jupyter installed. The notebook uses the following R packages:
- `tidyverse` (for data manipulation and visualization)
- `skimr` (for summary statistics)
- `DataExplorer` (for automated exploratory data analysis)

Install R and the required packages using the following commands:

```bash
# Install R (if not already installed)
# On Ubuntu/Debian:
sudo apt-get install r-base
# On macOS (using Homebrew):
brew install r
# Or download from https://cran.r-project.org/

# Install Jupyter
pip install jupyter

# Install R packages
R -e "install.packages(c('tidyverse', 'skimr', 'DataExplorer'), repos='http://cran.us.r-project.org')"
```

You’ll also need the IRkernel to run R in Jupyter:
```bash
R -e "install.packages('IRkernel', repos='http://cran.us.r-project.org'); IRkernel::installspec()"
```

## Usage
1. **Clone the Repository**  
   ```bash
   git clone https://github.com/your-username/video-game-sales-data-cleaning.git
   ```
2. **Navigate to the Directory**  
   ```bash
   cd video-game-sales-data-cleaning
   ```
3. **Launch Jupyter Notebook**  
   ```bash
   jupyter notebook
   ```
4. **Open the Notebook**  
   In the Jupyter interface, open [Video Game Sales Data Cleaning Notebook](video-game-sales-data-cleaning-in-r%20(1).ipynb).
5. **Run the Cells**  
   Execute the cells sequentially to:
   - Load the `vgsales.csv` dataset.
   - Inspect the data for missing values and structure.
   - Perform cleaning steps (impute missing values, remove duplicates, transform variables).
   - Save the cleaned dataset as `vgsales_cleaned.csv`.

**Note**: The dataset (`vgsales.csv`) is assumed to be in the `/kaggle/input/video-game-sales/` directory. If running locally, download it from [Kaggle](https://www.kaggle.com/datasets/gregorut/videogamesales) and place it in your project directory, or update the file path in the notebook.

## Data
The dataset (`vgsales.csv`) contains 16,598 records of video game sales with the following columns:
- **Rank**: Integer, game ranking by global sales.
- **Name**: Character, game title.
- **Platform**: Character, gaming platform (e.g., PS2, Wii).
- **Year**: Integer, release year.
- **Genre**: Character, game genre (e.g., Action, Sports).
- **Publisher**: Character, game publisher.
- **NA_Sales**, **EU_Sales**, **JP_Sales**, **Other_Sales**: Numeric, sales in millions for North America, Europe, Japan, and other regions.
- **Global_Sales**: Numeric, total sales worldwide in millions.

Source: [Kaggle Video Game Sales Dataset](https://www.kaggle.com/datasets/gregorut/videogamesales).

## Methodology
The cleaning process, implemented in the Jupyter Notebook, includes the following steps:
1. **Load Libraries and Data**: Use `tidyverse`, `skimr`, and `DataExplorer` to load and read `vgsales.csv`, treating "N/A" and empty strings as NA.
2. **Initial Inspection**: Examine dimensions, structure, and summary statistics to identify missing values, data types, and duplicates.
3. **Handle Missing Year Values**: Impute missing `Year` values with the median to support time-based analysis.
4. **Handle Unknown Publishers**: Replace "Unknown" in `Publisher` with NA and impute with the mode (most frequent publisher).
5. **Remove Duplicates**: Eliminate exact duplicate rows to ensure data integrity.
6. **Add Log-Transformed Sales**: Create `Global_Sales_log` using `log1p()` to handle skewness for visualizations.
7. **Convert to Factors**: Transform `Platform`, `Genre`, and `Publisher` to factors for categorical analysis.
8. **Handle Missing Sales**: Impute missing sales values (e.g., `NA_Sales`) with 0, assuming no sales recorded.
9. **Save Cleaned Data**: Export the cleaned dataset as `vgsales_cleaned.csv`.

## Output
The notebook produces:
- **Console Outputs**: Summary statistics, missing value visualizations, and histograms via `skimr` and `DataExplorer`.
- **Cleaned Dataset**: A file named `vgsales_cleaned.csv` with 11 original columns plus `Global_Sales_log`, no missing values, and no duplicates.
- **File Structure**: The cleaned CSV retains the original columns, with `Platform`, `Genre`, and `Publisher` as factors and imputed values for `Year` and sales.

## Contributing
Contributions are welcome! To contribute:
- Fork the repository.
- Create a new branch for your feature or bug fix.
- Submit a pull request with a clear description of your changes.

Please open issues for bugs, suggestions, or improvements.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments
- **Dataset**: The video game sales dataset is sourced from [Kaggle](https://www.kaggle.com/datasets/gregorut/videogamesales).
- **Libraries**: Thanks to the developers of [tidyverse](https://www.tidyverse.org/), [skimr](https://docs.ropensci.org/skimr/), and [DataExplorer](https://boxuancui.github.io/DataExplorer/) for their powerful R packages.

# ❤️ Health Data Analysis for Heart Disease

This Python project performs exploratory data analysis (EDA) on a health dataset to explore factors related to heart disease. Using Pandas for data cleaning and manipulation, Seaborn/Matplotlib for visualizations, and SciPy for statistical analysis, it investigates numerical features (e.g., BMI, SleepTime) and categorical features (e.g., Smoking, Sex) to uncover patterns. Ideal for learning data preprocessing, outlier handling, and correlation analysis.

## Features
- **Data Cleaning**: Handles missing values, removes duplicates, and trims excess rows for efficiency.
- **Outlier Handling**: Caps numerical outliers using z-scores to ensure robust analysis.
- **Feature Encoding**: Transforms categorical variables (e.g., GenHealth, AgeCategory) into numerical values for correlation analysis.
- **Exploratory Data Analysis (EDA)**: Visualizes distributions of numerical and categorical features, with box plots, histograms, and count plots.
- **Correlation Analysis**: Identifies highly correlated features using a heatmap to reduce redundancy.
- **Bivariate Analysis**: Explores relationships between categorical/numerical features and heart disease using box plots with hue.

## Interesting Techniques Used
Let’s dive into some key techniques in this project, step-by-step, like we’re sketching it out on a whiteboard. I’ll keep it clear and intuitive, so you can follow along even if you’re new to data analysis.

### 1. Outlier Handling with Z-Scores
To ensure numerical features like `BMI` or `SleepTime` don’t skew our analysis, we cap outliers using z-scores, keeping values within 3 standard deviations of the mean.

- **What’s happening?**: We calculate z-scores for each numerical column using `stats.zscore(df[column])`, which measures how many standard deviations a value is from the mean. If the absolute z-score is greater than 3 (indicating an outlier), we replace the value with `±3 * std + mean`, depending on the direction of the outlier.
- **How it works**: Suppose `BMI` has a mean of 28 and a standard deviation of 5. A BMI of 50 has a z-score of `(50 - 28) / 5 = 4.4`, which is > 3. We cap it at `3 * 5 + 28 = 43`. This keeps the data realistic without extreme values distorting our plots or correlations.
- **Why it’s cool**: Capping outliers preserves data points (unlike removing them) and reduces the impact of extreme values on visualizations and statistical analysis. It’s a gentle way to clean data while keeping its structure intact. Check out [SciPy’s zscore documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.zscore.html) for more.

### 2. Categorical Encoding for Correlation Analysis
Since correlation analysis requires numerical data, we transform categorical columns like `GenHealth` and `AgeCategory` into meaningful numerical values.

- **What’s happening?**: For `GenHealth`, we map categories to integers: `Poor: 0`, `Fair: 1`, `Good: 2`, `Very good: 4`, `Excellent: 5`. For `AgeCategory`, we assign ordinal values (e.g., `18-24: 0`, `25-29: 1`, up to `80 or older: 12`). Binary categories like `Sex` (`Male: 1`, `Female: 0`) and `Smoking` (`Yes: 1`, `No: 0`) are also encoded.
- **How it works**: Using `df.replace()`, we substitute string values with numbers. For example, a row with `GenHealth=Poor` becomes `GenHealth=0`. This allows us to include these columns in the correlation matrix, computed with `df.corr()`.
- **Why it’s cool**: Encoding categorical data as numbers lets us quantify relationships (e.g., does higher `AgeCategory` correlate with heart disease?). Ordinal encoding for `GenHealth` and `AgeCategory` preserves their natural order, unlike one-hot encoding. See [Pandas’ replace documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.replace.html) for details.

### 3. Correlation Heatmap for Feature Selection
We use a correlation heatmap to identify highly correlated features (correlation > 0.8) and remove them to reduce redundancy in future modeling.

- **What’s happening?**: We compute the correlation matrix for all numerical columns (including encoded categoricals) using `df.corr()`. We iterate through the matrix to find pairs with `abs(correlation) > 0.8`, adding the second feature of each pair to a set of features to drop. We then create a `reduced_df` without these features.
- **How it works**: If `PhysicalHealth` and `MentalHealth` have a correlation of 0.85, we add `MentalHealth` to `correlated_features`. The heatmap, plotted with `sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')`, shows correlations visually, with red for positive and blue for negative. Dropping correlated features simplifies the dataset without losing much information.
- **Why it’s cool**: Removing highly correlated features prevents multicollinearity in future models, improving their stability and interpretability. The heatmap makes these relationships instantly visible. Check out [Seaborn’s heatmap documentation](https://seaborn.pydata.org/generated/seaborn.heatmap.html) for more.

## Non-Obvious Libraries/Tools Used
- **[Pandas](https://pandas.pydata.org/)**: Handles data loading, cleaning, and encoding. Its `replace`, `drop_duplicates`, and `corr` functions are key for preprocessing and analysis.
- **[Seaborn](https://seaborn.pydata.org/)**: Simplifies visualizations with `boxplot`, `countplot`, `histplot`, and `heatmap`, supporting multi-dimensional analysis via `hue`.
- **[Matplotlib](https://matplotlib.org/)**: Customizes plot layouts (e.g., `plt.tight_layout()`) and creates subplots for multiple visualizations.
- **[SciPy](https://scipy.org/)**: Provides `stats.zscore` for outlier detection, enabling robust statistical preprocessing.
- **[NumPy](https://numpy.org/)**: Supports numerical operations, like computing z-score thresholds and array manipulations.

## Project Folder Structure

- **ids_project.py**: The main Python script with data cleaning, outlier handling, encoding, and EDA visualizations.
- **README.md**: This documentation file.

## Setup and Run
1. **Prerequisites**: Python 3.8+, pip, and a Jupyter environment (e.g., Google Colab or local Jupyter Notebook).

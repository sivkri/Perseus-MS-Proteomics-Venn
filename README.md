# Mass spectrometry Perseus Data analysis

This analysis will provide the steps to prepare the venn diagram of up and down regulated proteins obtained from Mass spectrometry Perseus.


# Repository Name

This repository contains code for performing data analysis and visualization using pandas and matplotlib libraries. The code processes data from different Excel files, applies filtering and merging operations, and generates venn diagrams based on the results.

## Installation

To use this code, you need to have Python installed. Additionally, you need to install the following dependencies:

- pandas
- matplotlib
- matplotlib_venn

You can install the dependencies using pip:

```
pip install pandas matplotlib matplotlib_venn
```

## Usage

1. Make sure you have the necessary dependencies installed.
2. Clone the repository to your local machine.
3. Place the input Excel files (`WT-DMSO_vs_WT-E20.xlsx`, `WT-DMSO_vs_HuRKO-DMSO.xlsx`, `HuRKO-DMSO_vs_HuRKO-E20.xlsx`) in the repository directory.
4. Open a Python environment or Jupyter notebook to run the code.
5. Import the required libraries:

```python
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib_venn import venn3, venn3_circles

# Set option to display all columns
pd.set_option('display.max_columns', None)
```

6. Load the data from the Excel files:

```python
wt_t = pd.read_excel("WT-DMSO_vs_WT-E20.xlsx")
ko = pd.read_excel("WT-DMSO_vs_HuRKO-DMSO.xlsx")
ko_t = pd.read_excel("HuRKO-DMSO_vs_HuRKO-E20.xlsx")
```

7. Perform data filtering based on significance:

```python
wt_t = wt_t[wt_t["Student's T-test Significant WT-DMSO_WT-E20"] == "+"]
ko = ko[ko["Student's T-test Significant WT-DMSO_HuRKO-DMSO"] == "+"]
ko_t = ko_t[ko_t["Student's T-test Significant HuRKO-DMSO_HuRKO-E20"] == "+"]
```

8. Perform data filtering based on intervals:

```python
wt_t = wt_t[wt_t["Student's T-test Difference WT-DMSO_WT-E20"].abs() > 1.0]
ko = ko[ko["Student's T-test Difference WT-DMSO_HuRKO-DMSO"].abs() > 1.0]
ko_t = ko_t[ko_t["Student's T-test Difference HuRKO-DMSO_HuRKO-E20"].abs() > 1.0]
```

9. Split the data based on up/down-regulation:

```python
wt_t_up = wt_t[wt_t["Student's T-test Difference WT-DMSO_WT-E20"] < -1.0]
wt_t_down = wt_t[wt_t["Student's T-test Difference WT-DMSO_WT-E20"] > 1.0]
ko_up = ko[ko["Student's T-test Difference WT-DMSO_HuRKO-DMSO"] < -1.0]
ko_down = ko[ko["Student's T-test Difference WT-DMSO_HuRKO-DMSO"] > 1.0]
ko_t_up = ko_t[ko_t["Student's T-test Difference HuRKO-DMSO_HuRKO-E20"] < -1.0]
ko_t_down = ko_t[ko_t["Student's T-test Difference HuRKO-DMSO_HuRKO-E20"] > 1.0]
```

10. Perform overlap analysis and generate venn diagrams:

```python
UP_MERGED = pd.merge(ko

.

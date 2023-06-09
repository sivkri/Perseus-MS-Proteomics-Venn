# Mass spectrometry Perseus Data analysis

This analysis will provide the steps to prepare the venn diagram of up and down regulated proteins obtained from Mass spectrometry Perseus.


# Perseus-MS-Proteomics-Venn

This repository contains a script that performs data analysis using the pandas and matplotlib libraries in Python. The script reads data from Excel files, filters and processes the data based on specific conditions, performs overlap analysis, and generates Venn diagrams. The resulting data and visualizations are saved to Excel files.

## Prerequisites

To use this code, you need to have Python installed. Additionally, you need to install the following dependencies:

- Python (version 3.6 or above)
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
UP_MERGED = pd.merge(ko_up, ko_t_up, on='Gene Symbol', how='inner')
DOWN_MERGED = pd.merge(ko_down, ko_t_down, on='Gene Symbol', how='inner')

# Get the overlapping genes
up_overlap = len(UP_MERGED)
down_overlap = len(DOWN_MERGED)
both_overlap = len(UP_MERGED.merge(DOWN_MERGED, on='Gene Symbol'))

# Generate venn diagram
venn_labels = {'10': up_overlap - both_overlap, '01': down_overlap - both_overlap, '11': both_overlap}
venn_diagram = venn3(subsets=venn_labels, set_labels=('WT-E20', 'HuRKO-DMSO', 'HuRKO-E20'))
venn_circles = venn3_circles(subsets=venn_labels, linestyle='dashed')

# Display the venn diagram
plt.title('Overlap of Differentially Expressed Genes')
plt.show()
```

Make sure you have the UP_MERGED and DOWN_MERGED DataFrames correctly generated based on the filtering steps mentioned earlier.

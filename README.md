### Overview¶
This notebook demonstrates the process of Netflix Data Cleaning, Analysis and Visulization. We'll walk through the following steps:
### About Dataset
Netflix is ​​a popular streaming service that offers a huge catalog of movies, TV shows, and original content. This dataset is a cleaned version of the original version. The data consists of content added to Netflix from 2008 to 2021. The oldest content dates back to 1925, and the newest is from 2021. This dataset will be cleaned using through a series of Python scripts. The purpose of this dataset is to test my data cleaning and visualization skills.
##### We are going to:
- Treat the Nulls
- Treat the duplicates
- Populate missing rows
- Use Grouping and Pivot Tables
- Visualize data

Extra steps and more explanation on the process will be explained through the code comments

### Motivation¶
Accurate content recommendation models are invaluable in the streaming industry, aiding:
Understanding audience preferences and content trends to inform content creation and acquisition strategies.
Identifying opportunities to improve user engagement and retention through data-driven insights.
Enhancing the user experience by providing personalized content recommendations based on viewing habits and preferences.
Optimizing content distribution and marketing efforts through data analysis and visualization.
Gaining a competitive edge in the streaming industry by leveraging data insights to drive business decisions.
### Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

* Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
* Pandas Library: This was used to analyze the data.
* Matplotlib Library: I visualized the data.
* Seaborn Library: Helped me create more advanced visuals.
* Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
* Visual Studio Code: My go-to for executing my Python scripts.
* Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.
#### Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.
### Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python 
# Importing Libraries
import warnings
import os

# Hide all warnings (including TqdmWarning and Kagglehub)
warnings.filterwarnings("ignore")
os.environ["PYTHONWARNINGS"] = "ignore"

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt  
import kagglehub

# Download latest version
path = kagglehub.dataset_download("ariyoomotade/netflix-data-cleaning-analysis-and-visualization")

# Check the file names in the folder (usually it's netflix1.csv)
print("Files in the folder:", os.listdir(path))

# Form the full path to the file and read it
csv_path = os.path.join(path, "netflix1.csv")
df = pd.read_csv(csv_path)

# Check the result
print(f"Rows loaded: {len(df)}")

# Data Cleanup
# Convert 'date_added' to datetime without specifying the exact format
df['date_added'] = pd.to_datetime(df['date_added'])
#Add mew column "year_added"
df['year_added'] = pd.to_datetime(df['date_added']).dt.year
```

### The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:


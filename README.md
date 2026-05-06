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

### Motivation
Accurate content recommendation models are invaluable in the streaming industry, aiding:
* Understanding audience preferences and content trends to inform content creation and acquisition strategies.
* Identifying opportunities to improve user engagement and retention through data-driven insights.
* Enhancing the user experience by providing personalized content recommendations based on viewing habits and preferences.
* Optimizing content distribution and marketing efforts through data analysis and visualization.
* Gaining a competitive edge in the streaming industry by leveraging data insights to drive business decisions.
### Tools I Used
For my deep dive into the data analyst streaming industry, I harnessed the power of several key tools:

* Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
* Pandas Library: This was used to analyze the data.
* Matplotlib Library: I visualized the data.
* Seaborn Library: Helped me create more advanced visuals.
* Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
* Visual Studio Code: My go-to for executing my Python scripts.
* Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.
### Data Preparation and Cleanup
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
Each Jupyter notebook for this project aimed at investigating specific aspects of the data streaming industry. 
### Visualize Data
#### Content Type Distribution (Movies vs. TV Shows)

```python
# Style settings
sns.set_theme(style='ticks')

# Create a grid of 1 row by 2 columns
fig, ax = plt.subplots(1, 2, figsize=(8, 4))

# Countplot 
sns.countplot(data=df, x='type', palette='viridis', ax=ax[0])
ax[0].set_title('Distribution of Content Type')
ax[0].set_xlabel('')
ax[0].set_ylabel('Number of Titles')
ax[0].invert_xaxis()
ax[0].yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y/1000:.1f}K'))

# Pie Chart
type_counts = df['type'].value_counts()
ax[1].pie(type_counts, labels=type_counts.index, autopct='%.0f%%', startangle=90)
ax[1].set_title('Percentage Distribution of Content by Type')

# Remove extra lines (despine) and align
sns.despine()
fig.tight_layout()

plt.show()
```
![Content Type Distribution (Movies vs. TV Shows)](images\Content_Type_Distribution.png)

*Bar Chart shows the absolute volume of titles for each category. Pie Chart shows the relative proportion of each content type within the whole library*

### Results:
The visualizations highlight that the platform’s library is heavily weighted toward films, with more than double the number of movies compared to television series.

### Visualize Data

#### Movies vs TV Shows Over Time

```python 
# Style settings
sns.set_theme(style='ticks')

sns.countplot(df, x='year_added', stat='count', palette='viridis')

plt.title('Content Added Over Time')
plt.xlabel('Release Year')
plt.ylabel('Number of Titles')
plt.xticks(rotation=45)

# Remove extra lines (despine) and align
sns.despine()
fig.tight_layout()
plt.grid(True, axis='y', linestyle='--', alpha=0.5)

plt.show()
```
![Movies vs TV Shows Over Time](images\Content_Added_Over_Time.png)
*This bar chart tracks the volume of titles added to a library based on their release year from 2008 to 2021*
### Results:
- **Stagnant Start (2008–2013):** For several years, the number of titles added remained near zero, indicating a very small or inactive catalog during this period.
- **Exponential Growth (2014–2019):** Starting in 2014, there is a massive upward trend. The volume increases rapidly year-over-year, jumping from a few hundred titles in 2016 to over 1,500 by 2018.
- **The Peak (2019):** The library reached its highest growth in 2019, surpassing 2,000 titles added in a single year.
- **Recent Decline (2020–2021):** There is a noticeable dip in 2020 and 2021. This could be due to production delays (often associated with the pandemic) or because the 2021 data was not yet complete when the chart was made
### Visualize Data
#### Content Addition Dynamics by Year and Type
```python
# Style settings
sns.set_theme(style='ticks')

fig, ax = plt.subplots(figsize=(8, 4))


sns.lineplot(data=df_plot, x='year_added', y='count', hue='type', marker='o', ax=ax)

# Adding values ​​above points
for t in df_plot['type'].unique():
    subset = df_plot[df_plot['type'] == t]
    for x, y in zip(subset['year_added'], subset['count']):
        ax.text(x, y + 4, f'{int(y)}', 
                ha='center', va='bottom', 
                fontsize=9, fontweight='semibold')


ax.legend(title='Type', loc='upper left')
ax.set_title('Type Of Content Added Over Time')
ax.set_xlabel('')
ax.set_ylabel('Number of Titles')

# Grid and layout
ax.grid(True, axis='y', linestyle='--', alpha=0.5)
sns.despine()
fig.tight_layout()

plt.show()
```
![Content Of Type Added Over Time](images\Type_of_Content_Added_Over_Time.png)

*This line graph tracks the number of Movies and TV Shows added to the Netflix platform between 2008 and 2021.*
### Results:
- **Movies vs. TV Shows:** Throughout the entire period shown, the platform consistently added more Movies than TV Shows.
- **Peak Year:** 2019 was the most prolific year for content additions across both categories.
- **Overall Shape:** The chart follows a "bell curve" shape, showing a rapid rise in content volume followed by a notable cooling-off period after 2019.
### Visualize Data
#### Release Year Distribution
```python 
# Style settings
sns.set_theme(style='ticks')

sns.histplot(df['release_year'],bins=40, kde=True)

plt.title('Release Year Distribution')
plt.xlabel('Release Year')
plt.ylabel('Number of Titles')
# Remove extra lines (despine) and align
sns.despine()
fig.tight_layout()
plt.grid(True, axis='y', linestyle='--', alpha=0.5)

plt.show()
```
![Release Year Distribution](images\Release_Year_Distribution.png)
*This chart is a histogram with an overlaid trend line (density plot) that shows how many titles on a platform were originally released in different years.*
### Results:
- **Modern Focus:** The data is heavily "left-skewed," meaning the collection is overwhelmingly dominated by movies and shows released in the last 10 to 15 years.
- **Growth Pattern:** The chart reflects the "Golden Age of Streaming," where production and acquisition of new content accelerated drastically during the 2010s.
### Visualize Data
#### Top 10 Countries with Most Content
```python 
top_10_countries = df['country'].value_counts().reset_index().head(10)
sns.set_theme(style='ticks')

# Top 10 Countries by Content

sns.barplot(data=top_10_countries, x='count', y='country', palette='viridis')

plt.title('Top 10 Countries by Content')
plt.xlabel('Number of Titles')
plt.ylabel('')

sns.despine()
plt.tight_layout()
plt.show()
```
![Top 10 Countries by Content](images\Top10_Countries_by_Content.png)
*This horizontal bar chart, illustrates which nations have produced the highest number of titles available on the platform.*
### Results:
- **U.S. Dominance:** The platform's library is heavily Western-centric, specifically favoring American productions.
- **Asian Influence:** There is a strong presence of Asian content, particularly from India, Japan, and South Korea, which combined make up a large portion of the international library.
### Visualize Data
#### Content Rating Distribution
```python 
fig, ax = plt.subplots(1, 2)
fig.set_size_inches((10, 4)) 

sns.set_theme(style='ticks')

# Distribution of Rating
plt.subplot(1, 2, 1)
sns.barplot(data=top_rating, x='count', y='rating', palette='viridis')

plt.title('Content Rating Distribution')
plt.xlabel('Number of Titles')
plt.ylabel('Rating Types')


plt.subplot(1, 2, 2)
plt.pie(top_rating['count'][:8], labels=top_rating['rating'][:8], autopct='%.0f%%', colors=sns.color_palette('viridis'), startangle=180)
plt.title('Percentage Rating Distribution')
plt.xlabel('')
plt.ylabel('')


sns.despine()
plt.tight_layout()
plt.show()
```
![Content Rating Distribution](images\Content_Rating_Distribution.png)
*This image contains two charts that provide a breakdown of the platform's library based on maturity ratings: a horizontal bar chart for total counts and a pie chart for percentages.*
### Results:
**Content Rating Distribution (Bar Chart)**

This chart shows the total number of titles per rating.
- **Most Common Rating:** TV-MA (Mature Audiences) is the most frequent rating, with over 3,000 titles.Runner-Up: TV-14 (Parents Strongly Cautioned) is the second most common, with over 2,000 titles.
- **Mid-Tier Ratings:** Ratings like TV-PG, R, and PG-13 fall into a middle range, with counts between roughly 500 and 900 titles.
- **Least Common:** Child-focused or niche ratings like TV-G and NR (Not Rated) have the smallest representation, with fewer than 250 titles each.

**Percentage Rating Distribution (Pie Chart)**

This chart illustrates the proportional share of the library's content.
* **Dominant Categories:** Combined, TV-MA (38%) and TV-14 (26%) make up nearly two-thirds (64%) of the entire library.
* **General/Family Content:** TV-PG (10%) and R (9%) follow, while younger audience ratings like PG-13 (6%), TV-Y7 (4%), and TV-Y (4%) represent much smaller slices of the pie.

**Summary** 

The data clearly shows that the platform's library is geared heavily toward mature and teen audiences, with adult-oriented content (TV-MA) being the single largest category.
### Visualize Data
#### Rating vs Content Type
```python 
# Style settings
sns.set_theme(style='ticks')

sns.countplot(df, x='rating', stat='count', palette='dark:red_r', hue='type', order=df['rating'].value_counts().index)

plt.title('Rating vs Content Type')
plt.xlabel('')
plt.ylabel('Number of Titles')
plt.xticks(rotation=90)

# Remove extra lines (despine) and align
sns.despine()
fig.tight_layout()
plt.grid(True, axis='y', linestyle='--', alpha=0.5)

plt.show()
```
![Rating vs Content Type](images\Rating_vs_Content_Type.png)
*This grouped bar chart, titled "Rating vs Content Type," compares the number of Movies (black bars) and TV Shows (red bars) across different maturity ratings*
### Results:
The chart highlights that while the platform is heavily weighted toward mature content (TV-MA/TV-14), it offers a much larger volume of Movies than TV Shows across nearly every rating category.
### Visualize Data
#### Top 10 Directors with the Most Titles
```python 
top_10_directors = df['director'].value_counts().reset_index()[1:10]
sns.set_theme(style='ticks')

# Top 10 Directors 

sns.barplot(data=top_10_directors, x='count', y='director', palette='viridis')

plt.title('Top 10 Directors')
plt.xlabel('Number of Titles')
plt.ylabel('')

sns.despine()
plt.tight_layout()
plt.show()
```
![Top 10 Directors ](images\Top10_Directors.png)
*This horizontal bar chart, titled "Top 10 Directors," ranks the individuals with the highest number of titles available on the platform*
### Results:
- **The Leader:** Rajiv Chilaka tops the list with exactly 20 titles.
- **Second Tier:** Alastair Fothergill and the duo Raúl Campos & Jan Suter follow closely, each with approximately 18 titles.
- **Middle Group:** Marcus Raboy and Suhas Kadav are tied with about 16 titles each, followed by Jay Karas with 14. 
- **Bottom of the List:** Cathy Garcia-Molina has 13 titles, while the well-known Martin Scorsese and Jay Chapman round out the top ten with 12 titles each.

**Summary:** Unlike the previous chart showing U.S. dominance, this list shows a diverse mix of international directors, including those known for animation (Chilaka, Kadav), nature documentaries (Fothergill), and stand-up specials (Raboy, Karas).
### Visualize Data
#### Top 10 genres in descending order
```python 
top_10_genres = df['listed_in'].value_counts().head(10).reset_index()
top_10_genres.columns = ['genre', 'count']

# Style settings
sns.set_theme(style='ticks')
plt.figure(figsize=(8, 4))

# Graph construction
ax = sns.barplot(
    data=top_10_genres, 
    x='count', 
    y='genre', 
    hue='count', 
    palette='viridis_r', 
    legend=False
)

# Adding numerical values ​​to the extremities
for i, v in enumerate(top_10_genres['count']):
    ax.text(v + 3, i, str(v), color='black', va='center', fontweight='bold')

plt.title('Top 10 Genres by Content Count', fontsize=12, pad=10)
plt.xlabel('Number of Titles')
plt.ylabel('')
plt.yticks(fontsize=8)

sns.despine()
plt.tight_layout()
plt.show()
```
![Top 10 Genres by Content Count](images\Top10_Genres_by_Content_Type.png)
*This horizontal bar chart, titled "Top 10 Genres by Content Count," displays the most common genre categories on the platform based on the number of titles.*
### Results:
- **The Top Three:** Dramas, International Movies leads with 362 titles, followed very closely by Documentaries with 359. Stand-Up Comedy takes the third spot with 334 titles.
- **Middle Tier:** Categories like Comedies, Dramas, International Movies (274) and Dramas, Independent Movies, International Movies (252) show a strong preference for multi-genre international content.
- **Family & Kids:** Kids' TV (219), Children & Family Movies (215), and the combination of Children & Family Movies, Comedies (201) represent a significant portion of the library.
- **The Bottom of the Top 10:** Documentaries, International Movies (186) and Dramas, International Movies, Romantic Movies (180) round out the list.

**Summary:**
The data shows that Dramas and International content are the most frequent labels, appearing in four of the top ten categories. Additionally, non-fiction content (Documentaries and Stand-Up) has a surprisingly high volume compared to specific fictional genres.
### Visualize Data
#### Top 10 Genres Market Share Analysis
```python
# Count the total number of original movies/series
total_titles = len(df)

# Count the number for each genre
genre_counts = df['listed_in'].value_counts().reset_index()
genre_counts.columns = ['genre', 'count']

# Add a column with a percentage of the TOTAL number of titles
genre_counts['percentage'] = (genre_counts['count'] / total_titles) * 100

# TOP Genres
top_10_genres = genre_counts.head(10)
print(top_10_genres)



import matplotlib.ticker as mtick

plt.figure(figsize=(8, 4))
ax = sns.barplot(data=top_10_genres, x='percentage', y='genre', palette='viridis')


ax.xaxis.set_major_formatter(mtick.PercentFormatter())

plt.title('Percentage of Total Content by Genre (Top 10)', fontsize=12, pad=10)
plt.xlabel('Percent of All Titles')
plt.ylabel('')
plt.yticks(fontsize=10)
sns.despine()
plt.show()
```
![Percentage of Total Content by Genre (Top10)](images\Top10_Genres_Market_Share.png)
*This horizontal bar chart, titled "Percentage of Total Content by Genre (Top 10)," breaks down the platform's library by what percentage each top genre represents.*
### Results:
While these are the "Top 10" genres, each individual category actually accounts for a relatively small slice (less than 5%) of the total library. This suggests the platform has a very diverse and fragmented catalog rather than being dominated by just one or two massive genres.

### Conclusion:

The charts collectively tell a story of rapid growth, a focus on modern global content, and a library geared heavily toward mature audiences. 

Here is a summary of the **key insights** across all the data provided:
-  **Growth and Timing.** 

*The Streaming Boom:* Content additions exploded between 2016 and 2019, following an exponential growth pattern.

*Recent Cooling:* Both the production of new titles and the addition of content to the platform peaked around 2018–2019 and have seen a slight decline since, possibly due to market saturation or pandemic-related production delays.
-  **Geographic and Content** 

*Focus U.S. Centrality with Global Reach*: While the United States is the primary content producer, the library has a massive international footprint, particularly from India and the UK.

*International Appeal:* "International Movies" appears as a label in four of the top ten genres, highlighting that global appeal is a core part of the platform's strategy. 

*Movies over TV:* Across almost all ratings and categories, Movies significantly outnumber TV Shows.

- **Audience DemographicsMature Leanings:** 

The library is dominated by TV-MA and TV-14 ratings, which together make up about 64% of all content.Niche for Families: While adult content is the most voluminous, there is a consistent "middle tier" of Children, Family, and Documentary content that provides variety beyond scripted adult dramas.

- **Di versity of Genres**

*No Single Dominant Genre:* Even the most popular genres (Dramas and Documentaries) only account for about 4% each of the total library. This indicates a highly fragmented and diverse catalog designed to offer "something for everyone" rather than specializing in one specific area.
# IBM Applied Data Science Capstone

## Predicting Falcon 9 First-Stage Landing Success

### Project Overview

SpaceX advertises Falcon 9 launches at a significantly lower cost than other
providers, largely because the first stage can be recovered and reused. If the
first stage can be expected to land successfully, the cost of a launch can be
estimated more accurately.

This project collects Falcon 9 launch records from the SpaceX REST API and from
Wikipedia, prepares them for analysis, explores them visually, geographically and
with SQL, and trains classification models to predict whether the first stage
will land successfully.

### Project Workflow

```
Data Collection (SpaceX API)
  -> Web Scraping (Wikipedia)
    -> Data Wrangling
      -> EDA (Visualization)
        -> SQL Analysis
          -> Interactive Visualization (Folium, Plotly Dash)
            -> Predictive Analysis
```

### Repository Contents

| File | Purpose | Stage |
| --- | --- | --- |
| [space-x-api-lab.ipynb](space-x-api-lab.ipynb) | Requests Falcon 9 launch, rocket, launchpad, payload and core records from the SpaceX v4 REST API and assembles them into a dataframe | 1. Data Collection |
| [space-x-web-scraping.ipynb](space-x-web-scraping.ipynb) | Scrapes the Falcon 9 and Falcon Heavy launch tables from a fixed Wikipedia revision using BeautifulSoup | 2. Web Scraping |
| [data-wrangling-lab.ipynb](data-wrangling-lab.ipynb) | Explores launch sites, orbits and landing outcomes, and derives the binary `Class` landing label from the `Outcome` column | 3. Data Wrangling |
| [eda-viz-lab.ipynb](eda-viz-lab.ipynb) | Visual exploration of flight number, payload mass, launch site, orbit and yearly trend against landing success, plus one-hot encoding of categorical features | 4. EDA Visualization |
| [eda-sql-lab.ipynb](eda-sql-lab.ipynb) | Loads the launch data into a SQLite database and answers the assignment questions with SQL queries | 5. EDA with SQL |
| [folium-lab.ipynb](folium-lab.ipynb) | Interactive map of launch sites with markers, marker clusters and distance lines to nearby coastline, railway and highway features | 6. Geographic Visualization |
| [plotly-dash.py](plotly-dash.py) | Plotly Dash application titled *SpaceX Launch Records Dashboard*, with a launch-site dropdown, success pie chart, payload range slider and payload vs. outcome scatter chart. Reads `spacex_launch_dash.csv`, which is provided by the course and is not included in this repository | 7. Interactive Dashboard |
| [predictive-analysis-lab.ipynb](predictive-analysis-lab.ipynb) | Standardises features, splits train and test sets, tunes four classifiers with grid search and compares their test accuracy | 8. Predictive Analysis |
| [IBM-Applied-ML-Capstone-Project-Report.pdf](IBM-Applied-ML-Capstone-Project-Report.pdf) | Final presentation covering all stages, results and conclusions | Summary |

### Methodology

**SpaceX API data collection.** Launch records are requested from
`api.spacexdata.com/v4`, with follow-up requests to the rockets, launchpads,
payloads and cores endpoints to replace identifiers with descriptive values. The
data is filtered to Falcon 9 launches and missing payload mass values are
replaced with the column mean.

**Web scraping.** A fixed Wikipedia revision of *List of Falcon 9 and Falcon
Heavy launches* is parsed with BeautifulSoup, the launch table headers are
extracted, and the table rows are parsed into a dataframe.

**Data wrangling.** Launches are counted by site and by orbit, landing outcomes
are tabulated, and the various outcome strings are mapped to a binary `Class`
label where 1 represents a successful first-stage landing.

**EDA and visualization.** Seaborn scatter, bar and line plots relate flight
number, payload mass, launch site, orbit type and launch year to landing
success. Categorical features are one-hot encoded for modelling.

**SQL analysis.** The dataset is loaded into a SQLite database and queried with
`ipython-sql` to summarise launch sites, payload totals by customer, landing
outcomes and date-bounded outcome counts.

**Folium geographic visualization.** Launch sites are marked on an interactive
map with circles, labels and clustered success or failure markers. Distances
from a launch site to nearby coastline, railway and highway points are measured
and drawn.

**Plotly Dash dashboard.** The application reads the course-provided
`spacex_launch_dash.csv`, a 55-launch extract with four launch sites
(`CCAFS LC-40`, `CCAFS SLC-40`, `KSC LC-39A`, `VAFB SLC-4E`). A dropdown selects
a single site or all sites and a slider sets a payload range from 0 to 10,000 kg,
driving a success pie chart and a payload versus outcome scatter chart through
callbacks. This extract is labelled differently and is smaller than the
90-launch dataset used elsewhere in the project, so the dashboard totals are not
expected to match the notebook figures.

**Predictive analysis.** Features are standardised with `StandardScaler` and
split into training and test sets with `train_test_split(test_size=0.2,
random_state=2)`, giving 18 test samples. Logistic regression, support vector
machine, decision tree and k-nearest neighbours classifiers are each tuned with
`GridSearchCV(cv=10)` and scored on the held-out test set.

### Predictive Analysis

Four classifiers were tuned by grid search with ten-fold cross-validation and
evaluated on the same 18-sample test set.

| Model | Test accuracy |
| --- | --- |
| Logistic Regression | 83.33% |
| Support Vector Machine | 83.33% |
| Decision Tree | 66.67% |
| K-Nearest Neighbors | 83.33% |

Three of the four models tie at 83.33% on the test set, so no single model can
be identified as best from this comparison. The test set contains only 18
samples, which is small enough that a difference of one prediction changes
accuracy by about 5.6 percentage points.

### Key Results

- 90 Falcon 9 launches make up the dataset used for exploratory and predictive
  analysis.
- Overall first-stage landing success across those launches is 66.67%.
- Landing outcomes vary across launch sites, payload mass, orbit type and launch
  year.
- Folium provides an interactive geographic view of the launch sites, and the
  Plotly Dash application provides an interactive dashboard over the separate
  55-launch course extract described above.

### Technologies

Python, pandas, NumPy, Requests, BeautifulSoup, Matplotlib, Seaborn, SQLite with
`ipython-sql`, Folium, Plotly, Dash, scikit-learn, Jupyter Notebook.

### Data Sources

- **SpaceX REST API v4**, `https://api.spacexdata.com/v4/` (launches, rockets,
  launchpads, payloads and cores endpoints).
- **Wikipedia**, *List of Falcon 9 and Falcon Heavy launches*, fixed revision
  `oldid=1027686922`.
- **IBM Skills Network course datasets** hosted on IBM Cloud Object Storage
  (`dataset_part_1.csv`, `dataset_part_2.csv`, `dataset_part_3.csv` and
  `spacex_launch_dash.csv`), used where a notebook loads a prepared snapshot.

### Presentation

[IBM-Applied-ML-Capstone-Project-Report.pdf](IBM-Applied-ML-Capstone-Project-Report.pdf)
is the final presentation, covering the project objective, each analysis stage,
the EDA, SQL, Folium and Dash results, the model comparison and the conclusions.

### How to Explore

1. Start with this README for the project scope and structure.
2. Read the notebooks in the stage order given in the Repository Contents table,
   beginning with `space-x-api-lab.ipynb` and ending with
   `predictive-analysis-lab.ipynb`. All notebooks are saved with their outputs,
   so they can be read on GitHub without being run.
3. To run the dashboard locally, install `dash`, `plotly` and `pandas`, place the
   course-provided `spacex_launch_dash.csv` in the repository root, then run
   `python plotly-dash.py` and open `http://127.0.0.1:8050`.
4. Read `IBM-Applied-ML-Capstone-Project-Report.pdf` for the summarised findings and conclusions.

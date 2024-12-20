# The Influence of History & Seasonality on Cinema: Analysing Movie Release Patterns by Genre, Theme, and Box Office Success Over Time

## Quickstart
```bash
# clone project
git clone https://github.com/epfl-ada/ada-2024-project-plottwisters.git
cd ada-2024-project-plottwisters

# [OPTIONAL] create conda environment
conda create -n <env_name> python=3.11 or ...
conda activate <env_name>


# install requirements
pip install -r requirements.txt
```

## Project Structure

The directory structure of the project looks like this:

```
├── Data                              <--- Initial raw data
│    ├── movie.metadata.tsv
│    ├── plot_summaries.txt
│    ├── TMDB_movie_dataset_v11.csv
│
├── generated                         <--- Initial data cleaned
│    ├── final_movie_dataset.csv
│
├──temporary                          <--- Files of the theme extraction
│    ├── preprocessed_data.csv
│    ├── merged_data.csv
│    ├── withplottone_data.csv
│    ├── df1.csv                      
│    ├── df2.csv
│    ├── df3.csv
│    ├── df4.csv
│    ├── df5.csv
│    ├── df6.csv
│    ├── df7.csv
│    ├── df8.csv
│    ├── df9.csv
│    ├── df10.csv                   
│
├── src                               <--- source codes
│    ├── 1_pre_processing.ipynb            (pre processing of the CMU dataset)
│    ├── 2_merge_datasets.ipynb            (add summaries to dataset and merge the TMDB on the CMU dataset)
│    ├── 3_plot_tone_extraction.ipynb      (add plot tone for each available summary)
│    ├── 4_theme_extraction.ipynb          (add theme for each available summary)
│
│
├── visuals
│    ├── 1.png
│    ├── 2.png
│    ├── 3.png
│    ├── 4.png
│    ├── 5.png
│    ├── ...
│    ├── df_1_histograms.png
│    ├── df_2_profbydecade.png
│    ├── df_3_profpermonth.png
│    ├── df_4_profdistribution.png
│    ├── df_5_proftrends.png
│    ├── df_6_proftrends20.png
│    ├── df_7_plot.html
│    ├── df_8_profheatmap.png
│
├── 1_preliminary_analysis.ipynb        <--- Preliminary xploration of the dataset
├── 2_analysis_plots.ipynb              <--- Analysis
├── 3_datastory_plots.ipynb             <--- Plots for the Datastory
├── 4_results.ipynb                     <--- Results of the analysis
│
├── requirements.txt
└── README.md
```

## Abstract
This project explores historical patterns in movie release timing, genre selection, thematic content, and box office performance using the CMU Movie Summary Corpus and supplemental data sources, including U.S. historical event timelines and box office metrics from the TMDB Movies Dataset. By aligning key dates and plot-driven themes with significant historical contexts, we aim to uncover how external factors influence both film profitability and audience reception.

We employ rigorous data preparation steps—cleaning metadata, adjusting financial figures for inflation, and standardizing attributes—to ensure consistent, time-spanning comparisons. Beyond traditional genre classifications, we apply ChatGPT-based theme extraction to enrich our understanding of thematic elements. These enhanced insights support the identification of optimal release windows for specific film types, reveal how audience preferences shift alongside historical developments, and offer strategic guidance on aligning content, timing, and themes for producers and distributors.

## Research questions
- Does each movie genre have an optimal release period (e.g. peak months or holiday periods) when audiences are more inclined to watch them, to maximise revenue?
  - Have popular release months shifted over time, and are these shifts related to changes in audience preferences, economic factors or competing media events?
  - How do thematic keywords in plot summaries correlate with the release dates and box office success of movies?

- How do significant historical events such wars, social movements or economic crises correlate with specific genres or themes prevalence over time, and how does it influence the box office performance ?
  - Do movies in certain countries cluster around specific times of year, potentially linked to cultural events or holidays?

- Do longer movies tend to be released at specific times compared to shorter ones?
  - What role does movie run time play in genre-based release strategies?
  - Does the length of a movie impact its box office success based on release timing?

## Additional Datasets

- TMDB Movies Dataset 2024 : There are many missing datapoints for box office returns in the CMU dataset (about 90%). Using the "Full TMDB Movies Dataset 2024 (1M Movies)" database from Kaggle, which contains data about over a million movies, would allow us to extract more data about not only box office returns but also viewer ratings which could be interesting to analyze. Linking this dataset to our primary dataset would be done through the name of the movie and year of release. 

## Methods 
To prepare the data for analysis, we began by isolating the columns relevant to our research: 'Wikipedia movie ID', 'Movie release date', 'Movie box office revenue', 'Movie runtime', 'Movie countries', and 'Movie genres'. Given the significant amount of missing data and raw formatting, standardizing the formatting through data cleaning was necessary. 

After further analysis, we focused on cleaning the following columns:
- **'Movie release date'**
  - **Issue**: Many movies lack a complete release date (day and month), and some are missing a release date entirely.
  - **Solution**: Create two new columns—one for the "year" and another for the full "year-month-day" format, with NaN values for missing information.

- **'Movie box office revenue'**
  - **Issue**: Box office values are difficult to compare across different years due to inflation and varying years of measurement.
  - **Solution**: Adjust all (USD) box office revenues that are not NaN to 2024 values using known US inflation rates, enabling consistent comparisons.

- **'Movie languages', 'Movie countries', 'Movie genres'**
  - **Issue**: These columns contain irregular characters and formatting (e.g., `"/m/02h40lc": "English Language"`).
  - **Solution**: Remove special characters and standardize the entries, providing each movie with a clean list of languages, countries, and genres. When the list is empty it means there is no data available.  

- **'Movie runtime'**
  - **Issue**: These columns contain unrealistic values (e.g., 18'000 hour long movie, 0sec long movie).
  - **Solution**: Filter out movies with unreasonable runtime.

This cleaning process ensures the dataset is structured and comparable across entries for accurate analysis.

Then, we need to find different algorithms or libraries to extract the themes and the plot tones from the movie summaries :

- **Movie themes**
  - We decided to use a ChatGPT API which allows us to extract one single theme from each movie summary. The prompt we give to the algorithm to extract the theme is : "Label the movie's theme with a single word based on its plot : ". Then, we clean the results to get rid of all the unwanted characters and store everything in the movie dataframe.

- **Plot tone analysis**
  - For this, we use the "VADER sentiment analysis" library which contains a dictionary. Each of the words inside are associated to a polarity score depending on how positive or negative they are. One total score is computed per summary, and then it is normalized and takes a value between -1 and +1 to categorize the text as positive, negative or neutral.

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
pip install -r pip_requirements.txt
```

## Project Structure

The directory structure of new project looks like this:

```
├── data                              <--- Initial data
│    ├── data_events.txt
│    ├── movie.metadata.csv
│    ├── plot_summaries.txt
│
├── generated                         <--- Initial data cleaned
│    ├── cleaned_data.csv
│    ├── usa_historical_events.csv
│
├──temporary
│    ├── df1_temporary.csv            <--- First part of the theme extraction
│
├── src                               <--- source codes
│    ├── pre_processing.ipynb
│    ├── theme_extract.ipynb
│    ├── plot_tone_extraction.ipynb
│
├── test
│    ├── notebook_draft.ipynb
│
├── analysis.ipynb                    <--- Results of the analysis
│
├── requirements.txt
└── README.md
```

## Abstract
This project analyzes the CMU Movie Summary Corpus to explore patterns in movie release timing, genre, themes, and box office success. Key research questions include identifying optimal release periods for different genres, examining shifts in popular release months over time, and investigating correlations between thematic content, historical events, and box office success. Supplementary data, including U.S. historical events and box office metrics from the TMDB Movies Dataset, will complete the data and able us to enhance our analysis accuracy. Data preparation involves cleaning essential columns (e.g., release date, runtime, box office revenue), adjusting financial figures for inflation, and standardizing movie attributes. To extract thematic and tonal elements from plot summaries, we use a ChatGPT API for theme classification and VADER sentiment analysis for sentiment analysis. These methods aim to uncover insights into audience preferences and release strategies, offering a historical perspective on movie success and timing within the film industry.

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
-  Historical events : Information about historical events are not available in the CMU dataset. This data gives us the type of histrocial event and when it happened in time. It contains only events that concerned the USA, knowing that most of the CMU dataset that we have contains american movies. This dataset was recreated by web scraping the data from : https://www.timetoast.com/timelines/us-history-in-the-20th-century. We preemptively verified that their data was copyright-free.

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

## Timeline
- 15.11.2024 P2 deadline: Data Handling and Preprocessing & Initial Exploratory Data Analysis.
- 29.11.2024 Preliminary analysis: sentiment analysis and themes extraction from the summaries, make first visuals and statistical tests to verify feasibility of hypotheses.
- 06.12.2024 Final Analysis: answer research questions with strong visuals and test that our results are statically relevant.
- 13.12.2024 Data story and first draft of the webpage.
- 20.12.2024 P3 deadline: Finalise visualisation and data story, clean code.

## Organization within the group
- Nicolas: Data handling and preprocessing, sentiment analysis 
- Elsa: Topic extraction, question 1
- Mentor: Topic extraction, question 1
- Lucie: Question 2, Clean code 
- Jack: Question 3, Set up webpage

## Questions for TA
- Is the ChatGPT API a good way to extract the themes ? We tried it and it works quite well, but we are not sure if it is the best way to do it. We also want to be sure that it is allowed.
- We decided that we want to combine the two datasets. What would be the best way of doing it ?

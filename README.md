# The Influence of History & Seasonality on Cinema: Analysing Movie Release Patterns by Genre, Theme, and Box Office Success Over Time

##Quickstart
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
### How to use the library
Tell us how the code is arranged, any explanations goes here.

## Project Structure

The directory structure of new project looks like this:

```
├── data                        <- Project data files
│
├── src                         <- Source code
│   ├── data                            <- Data directory
│   ├── models                          <- Model directory
│   ├── utils                           <- Utility directory
│   ├── scripts                         <- Shell scripts
│
├── tests                       <- Tests of any kind
│
├── results.ipynb               <- a well-structured notebook showing the results
│
├── .gitignore                  <- List of files ignored by git
├── pip_requirements.txt        <- File for installing python dependencies
└── README.md
```

## Abstract
## Research questions
- Does each movie genre have an optimal release period when audiences are more inclined to watch them, to maximise revenue?
  - Have popular release months shifted over time, and are these shifts related to changes in audience preferences or competing media events?
  - How do thematic keywords in plot summaries correlate with the release dates and revenue success of movies?

- How do significant historical events correlate with specific genres or themes prevalence, and how does it influence the box office performance ?
  - Do movies in certain countries cluster around specific times of year, potentially linked to cultural events or holidays?

- Do longer movies tend to be released at specific times compared to shorter ones?
  - What role does movie run time play in genre-based release strategies?
  - Does the length of a movie impact its box office success based on release timing?

(145 words)

## Additional Datasets
-  Historical events : Information about historical events are not available in the CMU dataset. This data gives us the type of histrocial event and when it happened in time. This dataset was recreated by web scraping the data from https://www.timetoast.com/timelines/us-history-in-the-20th-century. We preemptively verified that their data was copyright-free. 

## Methods 
To prepare the data for analysis, we began by isolating the columns relevant to our research: 'Wikipedia movie ID', 'Movie release date', 'Movie box office revenue', 'Movie runtime', 'Movie languages', 'Movie countries', and 'Movie genres'. Given the significant amount of missing data and raw formatting, standardizing the formatting through data cleaning was necessary. 

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



(249 words)

## Timeline
- 15.11.2024 P2 deadline: Data Handling and Preprocessing & Initial Exploratory Data Analysis.
- 29.11.2024 Preliminary analysis: sentiment analysis and themes extraction from the summaries, make first visuals and statistical tests to verify feasibility of hypotheses.
- 06.12.2024 Final Analysis: answer research questions with strong visuals and test that our results are statically relevant.
- 13.12.2024 Data story and first draft of the webpage.
- 20.12.2024 P3 deadline: Finalise visualisation and data story, clean code.

(71 words)

## Organization within the group
- Nicolas: Data handling and preprocessing, sentiment analysis 
- Elsa: Topic extraction, question 1
- Mentor: Topic extraction, question 1
- Lucie: Question 2, Clean code 
- Jack: Question 3, Set up webpage

(37 words)

## Questions for TA

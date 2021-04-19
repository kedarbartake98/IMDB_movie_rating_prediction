# IMDb movie rating prediction

## Problem Statement:

### Using publicly available IMDb data, build a model to predict a movie's IMDb rating.

##### Scope of the problem:
- **We use the data related to movies released after 2010 and before 2020 to build our model.**

**Dataset Source and short description:** [IMDb public data](https://www.imdb.com/interfaces/)

**Dataset files:**

The dataset uses two important keys to refer to titles and names of people. They are 'tconst' and 'nconst' respectively

1. **title.basics:** Contains basic information like titleName, genres, runtime, startYear, etc.
2. **title.ratings:** Contains IMDb rating information for titles.
3. **title.crew:** Contains list of directors and writers for each title
4. **title.principals:** Contains list of the cast for each title
5. **names.basics:** Contains basic information about a person like name, birthYear, deathYear, along with the titles they are most known for.
6. **title.akas:** Contains information on the various versions/languages/regions a title is released in. 

**Repository folders:**
- data : Contains dataset files
    - raw: files obtained from the web
    - interim: data saved after some initial processing like null value handling, missing value imputation, etc. and some primitive feature calculations.
    - processed: dataset in its final processed form with all features used to train models.
- notebooks : The process is divided into 5 steps each having their separate notebooks (listed in order)
    1. [Initial data processing](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Initial%20data%20processing.ipynb)
    2. [Feature Engineering](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Feature%20Engineering.ipynb)
    3. [Exploratory Data Analysis](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Exploratory%20Data%20Analysis.ipynb)
    4. [Data Sampling](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Data%20Sampling.ipynb)
    5. [Modeling](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Modeling.ipynb)
- models : directory to save trained models

### Approach:

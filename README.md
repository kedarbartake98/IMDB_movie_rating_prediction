# IMDb movie rating prediction

## Problem Statement:

### Using publicly available IMDb data, build a model to predict a movie's IMDb rating.

##### Scope of the problem:
- **We use the data related to movies released after 2010 and before 2020 to build our model.**

**Dataset Source and short description:** [IMDb public data](https://www.imdb.com/interfaces/)

**Dataset files:**

The dataset uses two important keys to refer to titles and names of people. They are _tconst_ and _nconst_ respectively

1. **title.basics:** Contains basic information like titleName, genres, runtime, startYear, etc.
2. **title.ratings:** Contains IMDb rating information for titles.
3. **title.crew:** Contains list of directors and writers for each title
4. **title.principals:** Contains list of the cast for each title
5. **names.basics:** Contains basic information about a person like name, birthYear, deathYear, along with the titles they are most known for.
6. **title.akas:** Contains information on the various versions/languages/regions a title is released in. 

**Repository folders:**
- **data** : Contains dataset files
    - **raw**: files obtained from the web
    - **interim**: data saved after some initial processing like missing value imputation, some primitive feature calculations, etc.
    - **processed**: dataset in its final processed form with all features used to train models.
- **notebooks** : The process is divided into 5 steps each having their separate notebooks (listed in order)
    1. [Initial data processing](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Initial%20data%20processing.ipynb)
    2. [Feature Engineering](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Feature%20Engineering.ipynb)
    3. [Exploratory Data Analysis](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Exploratory%20Data%20Analysis.ipynb)
    4. [Data Sampling](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Data%20Sampling.ipynb)
    5. [Modeling](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Modeling.ipynb)
- **models** : directory to save trained models

### Approach:

##### We frame the problem as a regression problem, the target being a movie's averageRating.
##### We describe the design choices and decisions taken in each step briefly:

#### Initial Data Processing [Notebook](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Initial%20data%20processing.ipynb):
In this stage, we process the data by imputing missing values, filtering rows in files to keep only those that are required to reduce computation, etc. We save the intermediate files in the interim data folder.

##### Handling null values:
1. **startYear:** We use a naive approach of assigning 0 to rows missing startYear. A description for a more sophisticated method for estimating the startYear is detailed in the notebook.
2. **genres:** The dataset has a comma-seperated string of atmost 3 genres associated with each title. For rows missing this field, we impute the genres based on the top 3 genres the cast and crew have worked on. Approach and implementation detailed in the notebook.

##### Filtering data:
1. Since we only consider the data of movies from 2010 to 2020, we discard data on other titles and also the people data that is associated with those other than the cast and crew of the considered titles.

#### Feature Engineering [Notebook](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Feature%20Engineering.ipynb):
The hallmark of this work is the engineered feature called the **Popularity Index**. We define the popularity index at two levels:
- popularity index of people
- popularity index of a title inferred from the popularity index of its cast and crew (actors/directors/writers)

Essentialy, the popularity index of a person aims to capture a summary of averageRatings of the titles that they have worked on in the past.
Currently, a simplistic approach is implemented in the notebook. Future scope and improvements/additions are described in the same.

Some other features are computed from the information available from various files in the dataset.

#### Exploratory Data Analysis [Notebook](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Exploratory%20Data%20Analysis.ipynb)
In this stage, we visualize the dataset and our engineered features to get insights on the same to inform our sampling strategy, feature engineering, etc.

#### Data Sampling [Notebook](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Data%20Sampling.ipynb)
- Data Sampling is important to make the claim that the model would work well even on unseen data based on its performance on test data.
- In classification problems, stratified sampling is used to split the train and test data.
- Since we have a regression problem at hand, we find the natural clustering of the data using KMeans algorithm and sample equally from each cluster to each of our train and test set.

##### Preprocessing 
- We preprocess the data in each of our feature columns according to the type of variables that it represents (categorical/numerical/ordinal).
- The notebook also contains a class for storing the parameters of preprocessing used in training for application in production

#### Model Building [Notebook](https://github.com/kedarbartake98/IMDB_movie_rating_prediction/blob/main/notebooks/Modeling.ipynb)
We train models on the processed data set to predict a movie's average rating

### Future Scope

**Engineering more features**: 
    - We can use columns like _numVotes_ to incorporate more information into the **Popularity Index**
    - **Time series of title ratings** for each person can be used to infer more features like their consistency and reliability of performance
    - In addition to a person working in a movie, we can establish the popularity of the **character** that they are playing - which might give additional insights on how popular the movie could be.
    - If we could have the text reviews of the titles, we could use **sentiment analysis** of the reviews as one more feature.
    - Also, the movie's performance in economic terms (gross profit, etc.) can be an important feature to infer the popularity of its cast.
    _Details in the Feature Engineering notebook_
    
**Use of External Data**:
    - Events in the actor's lives appearing on the web in news and **social media** can be used to infer the public's opinion on the person(actor/director/writer). This information can further be incorporated into the _Popularity Index_.
    - Other materials **(books, comics, etc.)** on the characters in the dataset might have **sales data** in the world. The sales on these materials can be associated with the particular character and be used to infer that character's popularity.
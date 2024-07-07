# Movie Recommendation System

This repository contains code for building a movie recommendation system for a couple.
Below is a detailed report outlining the methodology, experiments conducted, results obtained, and conclusions drawn from the project.

## 1. Data Collection and Preprocessing

### Data Sources:
- **MovieLens Dataset**: Used for movie metadata and user ratings.
- **IMDb Datasets (name.basics.tsv, title.basics.tsv)**: Additional movie metadata.

### Preprocessing Steps:
- **Cleaning and Merging**: Movie titles were processed to extract their release years. The IMDb datasets were then merged with the MovieLens dataset based on both the title and the year of release. If a movie from one dataset did not have an exact match in terms of both title and release year with a movie from the other dataset, it was not included in the merged dataset.

This merging approach ensured that only movies with identical titles and release years across both datasets were combined into a single entry. If there were movies with similar titles but different release years or vice versa, they were treated as distinct and not merged together.

## 2. Feature Engineering
- `Genres` were consolidated, and additional features like `isAdult`, `runtimeMinutes`, and `Year` were extracted and processed.

## 3. Model Development

### Collaborative Filtering (CF) with SVD:
- **Algorithm**: Singular Value Decomposition (SVD) was chosen due to its effectiveness with sparse data (many users rated only a few movies).
- **Implementation**: Surprise library was used for SVD model training and evaluation.
- **Feature Utilization**: This model only used rating as a feature

I tried using other model like KNN and and neural network, but KNN doesnt work well with sparse data and a neural network is more complicated to implement than SVD (it also needs a lot of data and we only have ~movies).

## 4. Recommendation Algorithm

My recommendation use the SVD model to predict a score from the user and combine this score with another score created from the feature `Genres` and `Year`
### Approach:
- **Feature Combination**: Created a `Features` column combining `Year` and `Genres`.
- **TF-IDF Vectorization**: Textual information in `Features` was converted using TF-IDF for similarity calculations.
- **User Profile Creation**: Constructed user profiles using rated movies to compute weighted averages of TF-IDF vectors.
- **Recommendation Generation**: Combined CF predictions (SVD) with CB scores (cosine similarity of user profiles) to generate personalized recommendations.

A Better approach would have been to create a model capable of incorporating additional features other than just ratings. However, due to the complexities involved, I decided instead to adjust the recommendation score slightly based on two main features: the movie's release year and its title.

## 5. Evaluation and Results

### Model Evaluation:
- **Cross Validation**: SVD model performance was evaluated using cross-validation on the MovieLens dataset.
- **Metrics**: RMSE and MAE were used to assess prediction accuracy.

### Results:
- **Recommendations**: The generated top movie recommendations for individual users (`user1`, `user2`) and pairs (`user1` and `user2` as a couple) seems to make sense based on the 2 users top movies.
- **Performance**: SVD model showed good results with low RMSE and MAE, indicating good prediction accuracy.

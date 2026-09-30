# 👗 Fashion Recommendation System

> **Data Science & Recommender Systems Project**
> Development of a personalized fashion recommendation system based on customer ratings and collaborative filtering techniques.

## 📌 Overview

This project focuses on the development of a **Fashion Recommendation System** designed to provide personalized clothing recommendations based on users' previous ratings.

The analysis uses the **ModCloth "Marketing Bias" dataset**, which contains information about clothing items, users, ratings, sizes, fit, categories and other product-related attributes.

The project combines **exploratory data analysis, collaborative filtering, hyperparameter optimization, clustering and matrix factorization** to analyze user preferences and generate personalized recommendations.

---

## 🎯 Objectives

The main objectives of the project are:

1. Explore and analyze customer ratings and clothing categories.
2. Investigate the distribution and variability of user ratings.
3. Develop a **K-Nearest Neighbors (K-NN)** collaborative filtering model.
4. Identify the optimal K-NN configuration through **Grid Search and Cross-Validation**.
5. Predict missing ratings and construct a completed user-item rating matrix.
6. Segment users according to their preferences using **K-Means clustering**.
7. Generate a personalized list of recommended items for each user.
8. Implement **Matrix Factorization using SVD** and compare its performance with K-NN.

---

## 🗂️ Dataset

The project uses the **Marketing Bias (ModCloth)** dataset.

The original dataset contains approximately:

* **99,893 observations**
* **12 variables**
* Nearly **100,000 customer ratings**

Relevant variables include:

| Variable     | Description                     |
| ------------ | ------------------------------- |
| `item_id`    | Identifier of the clothing item |
| `user_id`    | Identifier of the customer      |
| `rating`     | Customer rating                 |
| `timestamp`  | Date of the rating              |
| `size`       | Clothing size                   |
| `fit`        | Perceived fit                   |
| `user_attr`  | User characteristics            |
| `model_attr` | Clothing characteristics        |
| `category`   | Clothing category               |
| `brand`      | Brand                           |
| `year`       | Product year                    |

Missing values were removed during the preprocessing stage.

---

## 📊 Exploratory Data Analysis

The first stage of the project focused on understanding the structure and characteristics of the dataset.

The analysis included:

* Descriptive statistics
* Missing-value analysis
* Rating distribution
* Mean and median ratings
* Variance and standard deviation
* Median Absolute Deviation
* Rating distribution by clothing category
* Average rating per item
* Number of ratings per user and item

The overall average rating is approximately **4.21**, while the median rating is **5**, indicating a generally positive evaluation of the products.

The four main clothing categories analyzed were:

* 👗 Dresses
* 👚 Tops
* 👖 Bottoms
* 🧥 Outerwear

The average ratings across categories were relatively similar, with **Dresses** showing the highest average rating.

---

# 🤖 Collaborative Filtering with K-NN

The first recommendation approach is based on **K-Nearest Neighbors (K-NN)** collaborative filtering.

The model predicts a user's rating for an item by identifying users or items with similar rating patterns.

Two approaches were evaluated:

* **User-based**
* **Item-based**

Two similarity measures were also tested:

* **Pearson correlation**
* **Cosine similarity**

### 🔍 Hyperparameter Optimization

A **Grid Search** was performed to evaluate different combinations of:

* Number of neighbors (`K`)
* Recommender type
* Similarity metric

Values of `K` between **1 and 100** were investigated.

The models were evaluated using:

* **MSE — Mean Squared Error**
* **RMSE — Root Mean Squared Error**

### 🏆 Optimal Configuration

The best-performing configuration was:

```text
K = 5
Approach = User-based
Similarity = Pearson correlation
RMSE = 0.9456
MSE = 0.8945
```

A subsequent cross-validation produced:

```text
Mean RMSE = 0.9489
Mean MSE  = 0.9006
```

This configuration was then used to generate rating predictions and recommendations.

---

# ⭐ Rating Matrix Completion

The optimized K-NN model was used to construct a **user-item rating matrix**.

The matrix contains:

* Users as rows
* Clothing items as columns
* Known ratings where available
* Predicted ratings for missing values

A second matrix was created to keep track of whether each value was:

```text
0 → Original rating
1 → Predicted rating
```

This allowed the project to distinguish between observed and model-generated values during subsequent analysis.

---

# 👥 User Segmentation with K-Means

User preferences were further analyzed using **K-Means clustering**.

The objective was to identify groups of users with similar rating patterns.

The optimal number of clusters was investigated using the **Elbow Method**, based on the:

**Within-Cluster Sum of Squares (WCSS)**

Two different rating representations were compared.

The completed rating matrix produced a clear elbow at:

```text
K = 2
```

The users were therefore divided into two preference-based clusters.

This segmentation provides an additional perspective on customer behavior and can potentially be incorporated into future recommendation strategies.

---

# 🎯 Personalized Recommendations

Using the completed rating matrix, the system generates a list of the **Top-N recommended clothing items** for each user.

For each customer, the algorithm identifies items with the highest predicted ratings while distinguishing them from items for which an actual rating already exists.

The resulting recommendation structure can be represented as:

```text
User → Recommended Items → Predicted Rating
```

This allows the system to transform predicted preferences into personalized product recommendations.

---

# 🧮 Matrix Factorization — SVD

As an additional collaborative filtering approach, **Matrix Factorization** was implemented using **Singular Value Decomposition (SVD)**.

The method approximates the original user-item rating matrix through lower-dimensional representations of:

* Users
* Items

The model was optimized using **Grid Search** and evaluated through **5-fold Cross-Validation**.

The optimized SVD model was subsequently trained on the available dataset and used to predict missing ratings.

A separate matrix was created to distinguish between:

```text
Observed ratings
Predicted ratings
```

This approach provides an alternative to neighborhood-based K-NN recommendations and allows the performance of different collaborative filtering techniques to be compared.

---

# 🔬 Methodology

The overall workflow can be summarized as:

```text
                ┌──────────────────────┐
                │  ModCloth Dataset    │
                │ ~100k Ratings        │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ Data Preprocessing   │
                │ & Exploratory        │
                │ Data Analysis        │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ User-Item Rating     │
                │ Matrix               │
                └──────────┬───────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
        ┌─────────┐   ┌──────────┐   ┌─────────┐
        │   K-NN  │   │ K-Means  │   │   SVD   │
        │Filtering│   │Clustering│   │ Matrix  │
        └────┬────┘   └────┬─────┘   │ Factor. │
             │             │          └────┬────┘
             ▼             ▼               ▼
        ┌──────────────────────────────────────┐
        │ Rating Prediction & User Segmentation│
        └──────────────────┬───────────────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ Personalized Top-N   │
                │ Recommendations      │
                └──────────────────────┘
```

---

# 🛠️ Technologies & Libraries

The project was developed in **Python** using several data science and machine learning libraries.

| Technology       | Purpose                               |
| ---------------- | ------------------------------------- |
| **Python**       | Main programming language             |
| **Pandas**       | Data manipulation and preprocessing   |
| **NumPy**        | Numerical computation                 |
| **Scikit-learn** | K-Means, Grid Search and evaluation   |
| **Surprise**     | Collaborative filtering, K-NN and SVD |
| **Matplotlib**   | Data visualization                    |
| **Seaborn**      | Statistical visualization             |

---

# 📈 Evaluation

The recommendation models were evaluated using:

* **MSE**
* **RMSE**
* **Grid Search**
* **Cross-Validation**
* **Elbow Method**

The K-NN optimization identified a **user-based model with K=5 and Pearson similarity** as the best configuration among the tested alternatives, achieving an RMSE of **0.9456**.

The project also explored **SVD-based Matrix Factorization** as an alternative collaborative filtering technique, allowing different approaches to rating prediction to be compared.

---

# 🚀 Future Developments

Possible extensions of the project include:

* Combining **K-NN and SVD** into a hybrid recommendation system.
* Incorporating additional product attributes such as category, brand, size and fit.
* Developing content-based recommendations using clothing characteristics.
* Using implicit user behavior in addition to explicit ratings.
* Improving the clustering methodology with alternative similarity measures.
* Developing an interactive web interface for real-time recommendations.
* Applying more advanced matrix factorization and deep learning techniques.

---

## 📚 Project Context

This project was developed as part of a **Data Science / Recommendation Systems** academic project, with the objective of applying statistical analysis and machine learning techniques to a real-world recommendation problem.

The complete workflow covers the process from **data exploration and preprocessing to model optimization, user segmentation, rating prediction and personalized recommendations**.

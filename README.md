#  Coursera Course Recommendation System  
> **Final Project - Hybrid Filtering Model using TF-IDF + Content Features**  
> by **Amelia Hari Fauziah and Mamluatul Azazah from Group 6 SISTECH 2025**

A hybrid course recommendation system using content-based filtering with **TF-IDF** vectorization and **weighted scoring** based on rating, reviews, and course metadata. Built using Python and applied to real Coursera course data scraped and cleaned for machine learning.

---

##  Project Structure

```
Coursera_Scraping_Final Project 6.ipynb  # Jupyter Notebook containing the full pipeline
final_courses_with_cleaning.csv          # Preprocessed dataset with text-cleaned columns
```

---

## Objective

Build a recommendation engine that:

- Suggests top courses based on a user's **input keyword**
- Filters recommendations by **course level**, **duration**, and **certificate type**
- Ranks courses using a **hybrid scoring** method combining:
  - Text similarity (TF-IDF + cosine similarity)
  - Rating
  - Number of reviews

---

##  Tech Stack

- Python 3
- pandas, numpy
- scikit-learn (`TfidfVectorizer`, `cosine_similarity`)
- Jupyter Notebook

---

##  Workflow Explanation

### 1. **Data Loading**

```python
df = pd.read_csv("final_courses_with_cleaning.csv")
```
The dataset is a cleaned version of Coursera course listings including fields like title, skills, rating, reviews, certificate type, level, and duration.

---

### 2. **Text Cleaning**

Missing values in important text fields (`Title_clean`, `Skills_clean`, `Metadata_clean`) are filled with empty strings:

```python
df["Title_clean"] = df["Title_clean"].fillna("")
df["Skills_clean"] = df["Skills_clean"].fillna("")
df["Metadata_clean"] = df["Metadata_clean"].fillna("")
```

---

### 3. **TF-IDF Vectorization**

A new text column `combined_features` is created from `Title_clean` + `Skills_clean`:

```python
df["combined_features"] = df["Title_clean"] + " " + df["Skills_clean"]
```

Then vectorized using `TfidfVectorizer` from `sklearn` to transform text into numeric feature vectors:

```python
tfidf = TfidfVectorizer(stop_words="english")
tfidf_matrix = tfidf.fit_transform(df["combined_features"])
```

---

### 4. **Cosine Similarity**

Computes similarity between each course based on the TF-IDF matrix:

```python
cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)
```

---

### 5. **Feature Parsing**

####  Duration
Courses have varied formats like “1 week”, “2 hours”, “1 month”, which are parsed into week-equivalents:

```python
df["duration_weeks"] = df["Duration"].apply(parse_duration)
```

####  Reviews
Handles strings like “1.5K”, “2M”, and converts them into float values:

```python
df["num_reviews"] = df["Reviews"].apply(parse_reviews)
```

####  Rating
Ensures all ratings are numeric and fills missing with 0:

```python
df["Rating"] = pd.to_numeric(df["Rating"], errors="coerce").fillna(0)
```

---

##  Recommendation Logic

### Step-by-Step Hybrid Method:

1. **User inputs a keyword** (e.g. `"python data science"`).
2. Keyword is vectorized and cosine similarity is computed with all course vectors.
3. A **weighted score** is computed for each course:

```python
df["weighted_score"] = sim_scores * (df["Rating"] * np.log1p(df["num_reviews"] + 1e-5))
```

This score balances:
- Textual similarity
- High ratings
- More trustworthy courses (with more reviews)

4. Apply filters:
   - **Level**: e.g. `"Beginner"`
   - **Max duration**: e.g. 4 weeks
   - **Certificate type**: e.g. `"Professional Certificate"`

5. Return top N recommended results.

---

##  Example Usage

```python
results = hybrid_recommendations_filtered(
    keyword="python data science",
    level_filter="Beginner",
    max_duration_weeks=4,
    certificate_filter="Professional Certificate",
    top_n=10
)
print(results)
```

###  Sample Output

| Title | Rating | Reviews | Level | Duration | Certificate_Type | Weighted Score |
|-------|--------|---------|-------|----------|------------------|----------------|
| ...   | 4.8    | 25,000  | Beginner | 3 weeks | Professional Certificate | 35.12 |

---

##  Why Use TF-IDF?

- TF-IDF (Term Frequency-Inverse Document Frequency) gives **importance to unique terms**, reducing weight for common words like “course”, “learn”.
- It works well when course **titles and skills overlap** across categories.

---

##  Weighted Score Explanation

```python
weighted_score = cosine_similarity * (rating * log(num_reviews))
```

- This helps:
  - Boost highly-rated courses
  - Devalue courses with no ratings or few reviews
  - Rank results by **both relevance and quality**

---

## Limitations

- It’s **not collaborative**: it doesn't learn from user behavior, only content.
- Course durations are approximated.
- TF-IDF can't handle **semantic similarity** (e.g., “AI” vs “Artificial Intelligence”).

---

##  Future Improvements

- Use **word embeddings** (e.g., Word2Vec, BERT) for semantic understanding
- Add **collaborative filtering** to personalize results
- Include **user feedback or ratings history**
- Make a **streamlit/web app interface** for user input

---

##  Contact

> Made with love by **Amelia Hari Fauziah and Mamluatul Azazah**  
Feel free to connect for collaboration, research, or suggestions.

---

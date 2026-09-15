# Customer Engagement Analytics

**Customer engagement analysis, purchase behaviour prediction, and product recommendation system using Python and machine learning.**

---

## Project Overview

This project analyses e-commerce customer transaction data (417,534 events across real users) to understand purchasing behaviour, predict which customers are likely to buy, and recommend products — with a targeting system that predicts *when* a customer is likely to purchase next.

The work is split across two notebooks:

- **`AUC_Model.ipynb`** — Logistic Regression classifier for purchase prediction with ROC-AUC evaluation
- **`Customer_Engagement_Analytics.ipynb`** — Product recommendation engine, purchase date prediction, and user targeting system

---

## Dataset

- **Source:** `Customers_Transactions.xlsx` (train) + `Customers_Test_set.xlsx` (test)
- **Size:** 417,534 transaction-level events
- **Fields:** EventID, EventType (Purchased / Returned), ProductID, ProductName, Quantity, EventDateTime, UnitPrice, UserID
- **Event split:** 407,695 purchases | 9,839 returns

---

## Part 1 — Purchase Prediction Model (`AUC_Model.ipynb`)

### Feature Engineering
Aggregated transaction-level data to user-level features:
- `Total_Events` — total interactions per user
- `Views` — number of view events
- `AddToCart` — cart additions
- `Purchases` — purchase count
- `Total_Spend` — total spend per user
- `Purchased_Flag` — binary target (1 if user made any purchase)

### Model
- **Algorithm:** Logistic Regression (`sklearn`, max_iter=1000)
- **Split:** 80/20 train-test split (random_state=42)
- **Test set size:** 877 users

### Results

| Metric | Score |
|---|---|
| Accuracy | **1.00** |
| Precision (class 0) | 1.00 |
| Recall (class 0) | 1.00 |
| Precision (class 1) | 1.00 |
| Recall (class 1) | 1.00 |
| F1-Score | **1.00** |
| AUC | **1.00** |

**Confusion Matrix:**
```
Predicted →    0     1
Actual 0  [  12     0 ]
Actual 1  [   0   865 ]
```

### Feature Importance (Logistic Regression Coefficients)
| Feature | Coefficient |
|---|---|
| Total_Spend | 0.617 |
| Total_Events | 0.060 |
| Views | 0.000 |
| AddToCart | 0.000 |

`Total_Spend` was the strongest predictor of purchase behaviour.

### Outputs
- ROC Curve (AUC = 1.00)
- Confusion Matrix heatmap
- Feature coefficient table

---

## Part 2 — Recommendation & Targeting System (`Customer_Engagement_Analytics.ipynb`)

### Engagement Scoring
Assigned weights to event types to capture interaction intent:
- `Viewed` → 1 point
- `AddToCart` → 3 points
- `Purchased` → 5 points

### Product Recommendation
- Built a user-product score matrix using weighted event aggregation
- For each user, identified the product with the highest cumulative engagement score as their top recommendation
- Cold-start fallback: users with no history receive the most popular product

**Sample Output:**
```
UserID    Recommended_Product
25392     WATER BOTTLE
29856     ROSE GOLD VANITY MIRROR
```

### Purchase Date Prediction
- Identified each user's last activity date and first purchase date
- Calculated average days between first interaction and purchase
- Applied average lag to test set users to predict next purchase date

### Targeting System
- Built a function `get_target_users(product_name, top_n)` that returns the top N highest-engagement users for any given product, with their predicted purchase dates — enabling prioritised marketing outreach

**Sample Output for "ROSE GOLD VANITY MIRROR":**
```
UserID    Score    Predicted_Purchase_Date
30195     360.0    2020-12-02 15:27:00
```

### Final Output
Exported `Final_Predictions.xlsx` with UserID, Recommended_Product, and Predicted_Purchase_Date for all test users.

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python |
| Data Handling | Pandas, NumPy |
| Machine Learning | Scikit-learn (Logistic Regression, train_test_split, roc_curve, cosine_similarity) |
| Visualisation | Matplotlib, Seaborn |
| Environment | Jupyter Notebook |
| Data Files | Excel (.xlsx) |

---

## Author

**Samiksha Mankar**  
[LinkedIn](https://linkedin.com/in/samikshamankar) | [GitHub](https://github.com/mankar-samiksha15)

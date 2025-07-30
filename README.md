# Olist Customer Churn Prediction

##  Project Overview
Olist, a leading Brazilian e-commerce marketplace, experiences high customer acquisition but suffers from a **low repurchase rate of ~5%**. This project aims to **predict customer churn** using historical transaction, behavior, and delivery data, enabling proactive customer retention strategies through data-driven insights.

---

## Business Objective
- **Churn Definition**: A customer is considered churned if **no purchase occurred in the last 180 days**.
- **Goal**: Build a robust machine learning model to **predict churn likelihood** and identify key drivers behind customer disengagement.

---

## Data & Preprocessing
- **Source**: Olist’s transaction records (100,000+ orders)
- **Target Variable**:  
  - `churn = 1`: No purchase in past 180 days  
  - `churn = 0`: Active customer
- **Missing Values**: Imputed with median (e.g., review scores)
- **Normalization**: Yeo-Johnson transformation
- **Categorical Variables**: One-hot encoded
- **Train-Test Split**: 80% / 20%

---

## Feature Engineering

To better explore the association between customer churn and behavioral patterns, we engineered a comprehensive set of features derived from exploratory data analysis (EDA). These features aim to capture customer characteristics relevant to churn prediction and are grouped into three business-focused categories for improved interpretability:

### i. Customer Profile Features

| Feature                     | Description                                                                 | Justification & Potential Effect on Churn                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| `is_top5_state`             | Indicates whether the customer resides in one of the top 5 states by volume | Customers from key regions may benefit from better logistics and service quality, reducing churn risk     |
| `avg_payment_installments` | Average number of installments per purchase                                 | High installment usage may suggest financial caution, increasing churn likelihood if value perception drops |
| `most_common_payment_type` | Most frequently used payment method (credit card, boleto, etc.)             | Payment types reflect trust and engagement. Boleto users may indicate lower platform affinity              |
| `avg_review_score`         | Average product review score submitted by the customer                      | Higher scores reflect satisfaction; lower scores signal dissatisfaction and higher churn risk              |
| `num_reviews`              | Number of product reviews submitted                                          | More reviews suggest stronger engagement and emotional investment, typically reducing churn probability    |

### ii. Purchase Behavior Features

| Feature         | Description                                           | Justification & Potential Effect on Churn                                                                 |
|------------------|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| `num_orders`     | Total number of distinct purchases                    | Frequent buyers show loyalty and are less likely to churn                                                  |
| `total_spent`    | Total spending by the customer                        | High spenders are more invested financially and emotionally, decreasing churn risk                         |
| `avg_payment`    | Average order value (AOV)                             | Higher AOV implies greater confidence or interest in premium products, which correlates with retention     |

### iii. Order Detail Features

| Feature               | Description                                                            | Justification & Potential Effect on Churn                                                                |
|------------------------|------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| `avg_freight_value`    | Average shipping cost per order                                        | High shipping costs reduce perceived value, especially among price-sensitive users                        |
| `avg_product_weight`   | Average weight of products purchased                                   | May reflect product type (e.g., heavy items like furniture); could influence delivery time and churn      |
| `avg_approval_seconds` | Average time (in seconds) taken to approve orders                      | Faster approvals improve the purchase experience; delays may discourage future purchases                  |
| `avg_delivery_days`    | Average number of days between order and delivery                      | Longer delivery durations negatively impact satisfaction and trust                                        |
| `avg_delivery_performance` | Average difference between expected and actual delivery dates       | Consistent delays reduce customer trust and satisfaction, increasing churn risk                           |

---

## Model Development

Models evaluated:
- Logistic Regression (L2-regularized)
- K-Nearest Neighbors
- Decision Tree & Random Forest
- XGBoost & LightGBM
- Ensemble Models (Stacking)

**Evaluation Metrics**:
- Accuracy, Precision, Recall, F1-Score
- **Primary metric: ROC AUC**

---

## Final Model Selection

| Model                          | Test ROC AUC | Test F1 Score |
|-------------------------------|--------------|---------------|
| **Stacked (KNN5, LogReg, XGB)** | **0.9344**    | **0.8902**     |
| XGBoost                       | 0.9344       | 0.8900        |
| Stacked (LogReg, XGB)         | 0.9344       | 0.8899        |

> The **stacked ensemble** with KNN, Logistic Regression, and XGBoost was selected due to its **balanced performance, robust generalization**, and superior precision-recall trade-off on unseen data.

---

## Key Insights

- **Frequent and high-value buyers** are highly loyal and less likely to churn.
- **Delivery delays**, both in timing and approval speed, significantly reduce retention.
- **Low engagement** (fewer reviews, lower review scores) is associated with churn.
- **Top-5 state customers** benefit from better logistics and are more likely to stay.
- **Price-sensitive users** (e.g., those using Boleto or many installments) are more churn-prone.
- **Lighter product categories** may reflect casual or low-involvement purchases, leading to less repeat behavior.

---

## Recommendations

1. **Deploy Churn Prediction Model in Production**
   - Integrate into CRM workflows to trigger **real-time churn flags**
   - Prioritize retention campaigns for high-risk customers

2. **Improve Delivery Experience**
   - Reduce both actual delivery times and expectation gaps
   - Flag users with high `avg_delivery_performance` for proactive compensation or communication

3. **Engagement Programs**
   - Incentivize reviews to boost emotional investment
   - Reward users who consistently give positive feedback or shop frequently

4. **Tiered Retention Strategy**
   - High spenders: Exclusive offers, early access
   - Low activity users: Re-engagement discounts
   - Price-sensitive users: Installment or free shipping programs

5. **Geo-targeted Logistics Improvement**
   - Focus operational investment in states with high churn outside top-5 zones

---



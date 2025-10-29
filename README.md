# FraudFlow-MLOps: Cloud-Native End-to-End Fraud Detection

FraudFlow-MLOps is a cloud-native project built to detect fraudulent financial transactions using Google Cloud.  
It follows a clean, layered approach to turn raw data into meaningful insights and predictions — all inside the cloud.

---

## 🌐 What This Project Does

This project shows how data moves through a complete life cycle in a cloud setup.  
Starting from a raw CSV file, it passes through different layers: **Bronze, Silver, and Gold** , and ends with a trained AI model that can predict whether a transaction is fraudulent.

It’s called **MLOps** because every step is automated, consistent, and ready for scaling, just like production systems used in real companies.

---

## ☁️ Why Cloud-Native?

All the data is stored, cleaned, processed, and modeled inside **Google Cloud Platform (GCP)** using:
- **Google Cloud Storage** – to store the raw data file.
- **BigQuery** – to clean, transform, and prepare data using SQL.
- **Python and ML frameworks** – to train models.
- **Explainable AI (SHAP)** – to show why the model predicts a transaction as fraud.

Everything happens online without depending on a local computer.

---

## 🔄 Data Journey: From Raw to Intelligent

### 1. **Uploading the Data**
We begin with a CSV file containing millions of banking transactions.  
It is uploaded to **Google Cloud Storage**, which acts like a secure online folder.

### 2. **Bronze Layer — The Raw Zone**
The file is connected to **BigQuery** as an external table.  
This is where the data first enters the cloud system. It’s raw and untouched — just a mirror of the CSV.  
At this stage:
- We check how many rows exist.
- We confirm column names and data types.
- We look for missing or invalid values.

Think of Bronze as the “warehouse receiving area” : data arrives but isn’t unpacked yet.

---

### 3. **Silver Layer — The Clean Zone**
In the Silver layer, data is **cleaned, verified, and explored**.

Here we:
- Remove duplicates.
- Check for missing or incorrect values.
- Calculate simple statistics (minimum, maximum, and average amounts).
- Count how many unique senders, receivers, and transaction types exist.
- Analyze fraud distribution (for example, which transaction types are most risky).

We also explore **relationships** , like how balances change between senders and receivers, or which types of transfers are suspicious.

This layer ensures all data is accurate and ready for deeper analysis.  
Think of Silver as the “quality control room”.

---

### 4. **Gold Layer — The Smart Zone**
In the Gold layer, data is transformed into **features** that can teach a model how to detect fraud.  
We add new calculated fields such as:

| Feature | Meaning |
|----------|----------|
| `balanceDiffOrig` | Change in the sender’s balance (how much money actually left their account) |
| `balanceDiffDest` | Change in the receiver’s balance (how much money was received) |
| `isLargeTxn` | Whether the transaction amount is unusually high |
| `isTransferOrCashOut` | Flags risky transaction types like TRANSFER or CASH_OUT |

This becomes the “ready-to-learn” dataset.  
The Gold layer is where business logic and domain knowledge come together to create valuable insights.

---

### 5. **Machine Learning — Teaching the Model**
The clean, feature-rich data from the Gold layer is used to train multiple models that learn to recognize fraud.

We try several models:
- **Logistic Regression** – A simple and interpretable baseline.
- **Linear SVM** – Finds clear boundaries between fraud and non-fraud.
- **Random Forest** – Works by combining multiple small decision trees.
- **XGBoost and LightGBM** – Advanced models that learn fast and handle complex patterns.

Each model is trained, tested, and evaluated for accuracy.  
In this project, **Random Forest and LightGBM performed best**, reaching a ROC-AUC of nearly **0.995**, which is excellent.

---

### 6. **Explainable AI — Understanding the Model**
Once the model predicts fraud, we use **SHAP (SHapley Additive exPlanations)** to show **why** it made each decision.  
Instead of just saying “this is fraud”, the model explains which features caused that conclusion.

Key insights:
- Transactions involving **TRANSFER** or **CASH_OUT** are more likely to be fraudulent.
- Large **amounts** and big differences in balances signal higher risk.
- Normal transactions have consistent balance differences, while fraudulent ones often don’t.

This step ensures the system is **transparent and trustworthy**, which is vital for banking and finance.

---

## 📈 Results in Simple Words

- Fraud detection accuracy: **about 99% (ROC-AUC ≈ 0.995)**
- Balanced performance: catches almost all frauds while keeping false alarms low.
- Fully explainable: every prediction can be justified using SHAP.

The final model can effectively identify suspicious transactions with confidence.

---

## 🔮 Future Scope

This project can easily grow into a **real-time fraud detection system**.  
Possible next steps include:

1. **Streaming Integration** — Connect Kafka or Pub/Sub to stream transactions in real time.
2. **Automation with Airflow or Vertex AI** — Automatically retrain the model on new data every week.
3. **Drift Detection** — Track if model accuracy drops as new fraud patterns appear.
4. **Interactive Dashboards** — Show live fraud analytics in Looker Studio or Power BI.
5. **Alerts and Notifications** — Send messages when high-risk transactions occur.

With these upgrades, the project becomes a full production-grade MLOps pipeline.

---



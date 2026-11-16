# 🛒 Instacart Market Basket Analysis with PySpark & FP-Growth

An end-to-end distributed Market Basket Analysis project leveraging **Apache Spark (PySpark)** and the **FP-Growth** algorithm to extract frequent itemsets and association rules from millions of large-scale retail transactions.

---

## 📌 Technical Motivation: Why PySpark?
Standard single-node data processing libraries (like Pandas) struggle when handling massive transactional datasets due to memory constraints. 
* **The Scale:** The Instacart `order_products__prior` dataset contains **over 3.2 million orders** and tens of millions of individual order-item rows. 
* **The Solution:** **PySpark** was chosen to build a distributed data pipeline, allowing efficient memory management, parallelized ETL operations, and scalable MLlib association rule mining across clustered or optimized driver memory environments without risking `Out-Of-Memory (OOM)` errors.

---

## ⚙️ Architecture & Pipeline Workflow
1. **Distributed Data Ingestion:** Loading raw CSV files distributed across Spark's resilient distributed datasets (RDDs) / DataFrames with schema inference.
2. **ETL & Data Transformation:** 
   * Joining order-product relationships with product dictionary metadata.
   * Grouping items by `order_id` using distributed aggregations (`collect_set`) to construct transaction baskets.
3. **Frequent Pattern Growth (FP-Growth):** Utilizing Spark MLlib's parallelized FP-Growth implementation to mine frequent itemsets without generating candidate pairs explicitly.
4. **Rule Generation & Evaluation:** Extracting association rules filtered by robust thresholds (`minSupport`, `minConfidence`).

---

## 📐 Mathematical Foundations of Association Rules

Association rule mining identifies relationships between items in large transactional databases taking the form of $A \implies B$ (If item A is bought, item B is likely to be bought).

### 1. Support
Support represents the relative frequency (probability) that a set of items occurs together in the overall transaction database.
$$\text{Support}(A \implies B) = P(A \cap B) = \frac{\text{Count}(A \cup B)}{\text{Total Transactions}}$$

### 2. Confidence
Confidence measures the reliability or conditional probability of the rule—what proportion of transactions containing item $A$ also contain item $B$.
$$\text{Confidence}(A \implies B) = P(B \mid A) = \frac{\text{Support}(A \cup B)}{\text{Support}(A)}$$

### 3. Lift
Lift measures how much more often items $A$ and $B$ occur together than expected if they were statistically independent.
$$\text{Lift}(A \implies B) = \frac{P(B \mid A)}{P(B)} = \frac{\text{Support}(A \cup B)}{\text{Support}(A) \times \text{Support}(B)}$$

* **$\text{Lift} > 1$**: Positive correlation (items appear together more frequently than expected by chance).
* **$\text{Lift} = 1$**: Statistical independence (no relationship).
* **$\text{Lift} < 1$**: Negative correlation (substitutable items).

> *Note on Evaluation:* High confidence alone can be misleading if the consequent item ($B$) has a very high baseline frequency across all transactions. **Support, confidence, and lift must be evaluated together** to isolate genuinely actionable cross-selling rules.

---

## 🛠️ Tech Stack & Libraries
* **Distributed Computing:** Apache Spark, PySpark SQL, PySpark MLlib (`FPGrowth`)
* **Data Processing & Analysis:** Pandas, NumPy
* **Visualization:** Matplotlib
* **Environment:** Jupyter Notebook / Google Colab

---

## 🚀 How to Run the Notebook
1. Open the `Association_Rule_Learning.ipynb` notebook in **Google Colab** or Jupyter.
2. Run the initialization cell to set up Kaggle credentials and pull the Instacart dataset automatically.
3. Execute the Spark session configuration (optimized with custom driver memory settings) and step through the pipeline.

---

## 👤 Author
**Yassine Barigou**

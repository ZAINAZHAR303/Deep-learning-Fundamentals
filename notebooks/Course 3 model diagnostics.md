# 📊 ML Model Diagnostics Cheat Sheet

> **Scenario Metrics:**
> - **Train Accuracy:** $95\%$ (Train Error: $5\%$)
> - **Dev Accuracy:** $88\%$ (Dev Error: $12\%$)
> - **Test Accuracy:** $74\%$ (Test Error: $26\%$)

---

## Step 1: Identify the Gaps & Implications

| Gap | Metric / Delta | Implication |
| :--- | :--- | :--- |
| **Avoidable Bias** | $\text{Human-Level Error (HLE)} - \text{Train Error}$ | Measures how well the model learns training data relative to optimal human performance. |
| **Variance / Data Mismatch Gap** | $5\% \to 12\%$ **($7\%$ Gap)** | **If same distribution:** High Variance (Overfitting).<br>**If different distributions:** Needs a **Train-Dev Set** to isolate Variance from Data Mismatch. |
| **Dev-to-Test Overfitting Gap** | $12\% \to 26\%$ **($14\%$ Gap)** | Massive drop indicates **overfitting to the dev set** (from excessive hyperparameter tuning) or an invalid split where dev/test distributions differ. |

---

## Step 2: Prioritized Action Plan (What to do FIRST)

### 1. Establish Human-Level Error (FIRST STEP)
Evaluate human performance on the task to determine optimal Bayes Error:
* **If HLE is $1\%$:** Avoidable Bias is $4\%$ $\to$ Focus on increasing model capacity / architecture before fixing variance.
* **If HLE is $5\%$:** Avoidable Bias is $0\%$ $\to$ Skip training error entirely; focus on variance and data mismatch.

### 2. Isolate Data Mismatch vs. Variance
Create a **Train-Dev Set** (data drawn from training distribution, excluded from backpropagation):
* **If $\text{Train-Dev Error} \approx 12\% \to \textbf{High Variance:}$**
  * Apply regularization (Dropout, $L_2$).
  * Collect more training data.
  * Simplify model architecture.
* **If $\text{Train-Dev Error} \approx 5\% \to \textbf{Data Mismatch:}$**
  * Collect more training data matching the dev/test distributions.
  * Perform artificial data synthesis / data augmentation.

### 3. Resolve the Dev-to-Test Gap
* **Rule:** Dev and Test sets **must** come from the exact same distribution.
* **Action:** If distributions already match, a $14\%$ gap means hyperparameters were over-fit to the dev set $\to$ Obtain a fresh, larger Dev set and re-evaluate.
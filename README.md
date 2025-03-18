# 🏠 Property Price Prediction - Cook County  
*Enhancing real estate valuation using data analytics and machine learning*  

## 📌 Project Overview  
This project aims to refine the **property valuation process** in **Cook County, Illinois**, addressing issues of **transparency and efficiency** in real estate assessments. Using **machine learning techniques in R**, we developed predictive models to estimate property sale prices accurately.  

By leveraging **historic property data**, we analyzed real estate patterns and built a robust model validated on a separate dataset to ensure **real-world applicability**.

---

## 📂 Repository Structure  
```
📦 Property-Prediction  
 ┣ 📂 data              # Training and test datasets  
 ┣ 📂 notebooks         # Jupyter notebooks for EDA and modeling  
 ┣ 📂 scripts          # R scripts for data preprocessing and training  
 ┣ 📂 models           # Trained models and results  
 ┣ 📜 README.md        # Project documentation  
 ┗ 📜 requirements.txt  # Dependencies  
```

---

## 🔍 Data Sources  
We used two datasets for training and validation:  
- **Training Data**: `historic_property_data.csv`  
- **Test Data**: `predict_property_data.csv`  

Each dataset includes features such as:  
- **Property attributes**: Size, bedrooms, bathrooms, age, and structure details  
- **Geographical information**: Town & Neighborhood codes, proximity to schools and roads  
- **External influences**: Noise indicators, floodplain risks, and tax rates  

---

## ⚙️ Installation  
Clone the repository:  
```sh
git clone https://github.com/BalakrishnanIyer/Property-Prediction.git
cd Property-Prediction
```
Install required R dependencies:  
```r
install.packages(c("tidyverse", "glmnet", "leaps", "rpart", "rpart.plot", "mice"))
```

---

## 🛠 Data Preprocessing  
### ✅ Feature Engineering & Selection  
- **Removed non-predictive columns** to enhance model performance  
- **Converted categorical variables** into factor types for better analysis  
- **Addressed missing values**:  
  - **Dropped** columns with >10% missing values  
  - **Imputed** missing values using **Predictive Mean Matching (PMM)**  
- **Handled outliers** with the **Z-score method** to maintain data consistency  

---

## 📈 Model Development  
We tested various **regression models**:  

### 🔹 Traditional Regression Methods  
1️⃣ **Forward Selection** - Added predictors step-by-step to improve accuracy  
2️⃣ **Backward Elimination** - Started with all predictors, removing the least significant  
3️⃣ **Stepwise Regression** - Combined forward and backward selection  

> *💡 These methods resulted in higher Mean Squared Error (MSE), suggesting multicollinearity issues.*  

### 🔹 Lasso Regression (Final Model)  
✅ Applied **L1 regularization** to shrink coefficients and improve generalization  
✅ Used **10-Fold Cross-Validation** to optimize the **lambda (λ)** parameter  
✅ Selected **best lambda** to balance model complexity and accuracy  

#### 🚀 Results:  
- **Lowest MSE achieved**: `15,903,578,320`  
- **Feature Reduction**: Retained only the most impactful variables  
- **Improved Performance**: Enhanced interpretability and reduced computational load  

---

## 🚀 Prediction Process  
To use the trained model for **property price predictions**:  

```r
# Load necessary libraries
library(glmnet)

# Load test dataset
test_data <- read.csv("predict_property_data.csv")

# Preprocess test data
x_test <- model.matrix(~., test_data)[,-1]

# Load trained Lasso model
lasso_fit <- readRDS("models/lasso_model.rds")

# Make predictions
preds <- predict(lasso_fit, s=best_lambda, newx=x_test)

# Save results
write.csv(data.frame(Property_ID = test_data$pid, Predicted_Price = preds), "predicted_prices.csv", row.names = FALSE)
```

---

## 🔮 Future Enhancements  
🔹 **Hyperparameter tuning** for improved model precision  
🔹 **Deploying the model** via a web API (Flask/FastAPI)  
🔹 **Exploring other machine learning methods** (e.g., Random Forest, Gradient Boosting)  

---

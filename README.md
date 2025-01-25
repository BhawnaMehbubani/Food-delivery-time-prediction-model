#  **Delivery Time Prediction System**  

Welcome to the **Delivery Time Prediction System**, a comprehensive machine-learning solution designed to predict food delivery times with precision and efficiency. This project is a testament to detail-oriented development, integrating advanced machine learning techniques with robust preprocessing pipelines and real-world data.  


##  **What is this project about?**
This project predicts **Estimated Delivery Time (ETA)** for food orders based on:  
- Order details (e.g., restaurant and delivery addresses).  
- Delivery agent attributes (e.g., ratings, vehicle condition).  
- External factors (e.g., traffic, weather, distance).  

The goal is to improve **customer satisfaction**, **logistics efficiency**, and **operational predictability** in delivery systems.  


##  **Why was this project built?**
### **1. Solving a Real-World Problem**  
Logistics is the backbone of modern e-commerce and food delivery. Predicting accurate ETAs ensures:  
- Better **customer experience** by setting realistic expectations.  
- Improved **delivery agent allocation** for businesses.  

### **2. Applying Advanced Machine Learning Techniques**  
This project was an opportunity to apply and understand:  
- **Regression algorithms** for time prediction.  
- **Geospatial distance computation** using geocoding APIs.  
- **Pipeline creation for preprocessing and feature engineering**.  

### **3. Building an End-to-End Solution**  
From gathering raw user inputs to making accurate predictions, this project showcases **seamless integration of ML with web development**.  


## **How does it work?**

### **High-Level Architecture**  
Here’s the **end-to-end workflow** for the system:  

```plaintext
+-----------------------------------+
|         User Interface (UI)       |  
|      (Streamlit Application)      |  
+-----------------------------------+
                 ↓
+-----------------------------------+
|        Backend & Preprocessing    |  
| - Geocoding with OpenCage API     |  
| - Feature Engineering             |  
| - Distance Calculation (Geopy)    |  
+-----------------------------------+
                 ↓
+-----------------------------------+
|      Machine Learning Model       |  
| - Trained Regression Model        |  
| - Feature Scaling (StandardScaler)|  
| - Prediction                      |  
+-----------------------------------+
                 ↓
+-----------------------------------+
|          Output Display           |  
| - Estimated Delivery Time (ETA)   |  
+-----------------------------------+
```

### **Workflow Pipeline in Detail**  

#### **1. Input Collection (Frontend)**  
- Built with **Streamlit**, the app collects the following user inputs:  
  - **Order Details**: Restaurant address, delivery address, order date, and time.  
  - **Delivery Agent Info**: Age, ratings, vehicle condition, etc.  
  - **Environmental Factors**: Weather conditions, traffic density, city type.  

#### **2. Geocoding (Address to Coordinates)**  
- The **OpenCage API** converts addresses into precise latitude and longitude coordinates.  
- Why OpenCage?  
  - Provides reliable geocoding services with support for edge cases (e.g., incomplete addresses).  
  - Easy integration with Python and detailed responses.  

#### **3. Feature Engineering**  
- **Date-Time Features**:  
  Extracted features such as:  
  - Day, month, year, day of the week, weekend indicator, and quarter.  
  - Special flags for month/quarter/year start and end dates.  

- **Geospatial Distance**:  
  Using **Geopy**, the app calculates the distance (in kilometers) between the restaurant and delivery locations.  

- **Order Preparation Time**:  
  Time difference between order placement and pickup is computed and imputed where necessary.  

- **Categorical Encoding**:  
  Categorical variables (e.g., weather, traffic, order type) are label-encoded for compatibility with ML models.  

#### **4. Data Scaling**  
- **StandardScaler**:  
  - Used for scaling numerical data to ensure features with different magnitudes (e.g., distance, ratings) are treated equally by the model.  
  - Chosen for its efficiency and compatibility with regression algorithms.  



## **Machine Learning Model**

### **1. Chosen Algorithm: Random Forest Regressor**

The **Random Forest Regressor** was the optimal choice for predicting delivery times, and here's why:  
- **Robustness to Non-Linearity**:  
  The relationship between features like weather, road traffic density, distance, and delivery time isn't linear. For example:
  - On a rainy day (weather), deliveries are delayed disproportionately in high traffic areas.
  - Delivery distances affect travel times differently depending on road density and vehicle type (e.g., a bike vs. a truck in traffic jams).  
  The Random Forest Regressor excels at handling such complex, non-linear relationships by splitting data across multiple decision trees and averaging their results.  

- **Feature Importance Analysis**:  
  Random Forest not only predicts delivery times but also highlights which features are most critical. For instance, during training:
  - Features like `distance`, `road traffic density`, and `weather conditions` contributed the most to accurate predictions.
  - Insights: Weather and traffic density increased delivery time variance by **20%** during peak hours.

- **Performance on Tabular Data**:  
  The dataset was structured with numerical and categorical columns like ratings, city type, and vehicle condition. Random Forest performs exceptionally well on such tabular data due to its inherent ability to handle both types of features.


### **2. Why Not Other Algorithms?**

#### **Linear Regression**  
- **Why Not?**  
  Linear Regression assumes a direct, linear relationship between inputs and outputs.  
  - Example: It would assume that a 10 km delivery in heavy traffic takes proportionately 10x longer than in no traffic. However, real-world delays don't scale linearly with distance due to factors like road bottlenecks.  
  - This led to significant underfitting, with an RMSE of **15.7 minutes**, nearly double that of Random Forest.  

#### **Gradient Boosting/XGBoost**  
- **Why Considered?**  
  Gradient Boosting models, especially XGBoost, are highly accurate for tabular data and perform well on non-linear relationships.  
- **Why Not Used?**  
  - **Training Times**: XGBoost took **3x longer** to train compared to Random Forest, which was crucial given the project's emphasis on quick deployment.  
  - **Marginal Gains**: The accuracy improvement was marginal (RMSE dropped by only **0.1 minutes**) compared to Random Forest, making it less suitable for this phase.


### **3. Model Training and Metrics**

#### **Dataset**  
- The cleaned dataset contained features like `delivery person's age`, `distance`, `weather`, `traffic density`, `ratings`, and `vehicle condition`.  
- To ensure accuracy:
  - Null values (e.g., missing timestamps) were handled with medians.
  - Features like `distance` and `time differences` were engineered using geodesic calculations and timestamp processing.

#### **Metrics Used**  
- **Root Mean Squared Error (RMSE)**: Focused on penalizing larger errors more heavily (important for precise predictions).  
- **Mean Absolute Error (MAE)**: Provided an average estimation of errors.  

#### **Final Results**  
- **Random Forest**:
  - RMSE: **8.23 minutes**  
  - MAE: **6.45 minutes**  

- **Linear Regression**:
  - RMSE: **15.7 minutes**  
  - MAE: **12.2 minutes**  

- **XGBoost**:
  - RMSE: **8.13 minutes**  
  - MAE: **6.32 minutes**  

### **4. Special Observations**
- **Trend Analysis**:
  - Deliveries during rainy and high-traffic conditions took **25% longer** on average, emphasizing the importance of these features in prediction.
  - Distance below **5 km** showed significant deviations in predictions if traffic density was omitted.
- **What If Another Algorithm Was Used?**  
  - Using **Linear Regression** would lead to underestimations for long-distance and high-traffic deliveries, as it doesn't capture their compounding effects.
  - Switching to **Gradient Boosting/XGBoost** would slightly enhance accuracy but with a trade-off in speed and resource efficiency.

#### **4. Key Features Driving Predictions**  
Based on feature importance from Random Forest:  
1. **Distance between restaurant and delivery location**.  
2. **Order preparation time**.  
3. **Traffic density**.  
4. **Weather conditions**.  
5. **Time of order placement**.  


##  **Observations**  
After completing the project and analyzing the results, we identified several trends and observations:  

1. **Distance is a Critical Factor**:  
   - Orders with longer distances consistently take more time, even with minimal traffic.  

2. **Traffic and Weather Impact**:  
   - **High traffic density** increases delivery time by 15-30% on average.  
   - Adverse weather conditions (e.g., rain or fog) delay deliveries by an additional 10-20%.  

3. **Peak Hours vs. Off-Peak**:  
   - Orders during **peak hours** (12 PM - 2 PM, 6 PM - 9 PM) take significantly longer compared to off-peak hours.  

4. **Delivery Agent Experience**:  
   - Agents with higher ratings and better vehicle conditions tend to complete deliveries faster.  

5. **Order Preparation Times Vary**:  
   - Some order types (e.g., buffets or meals) take longer to prepare, significantly impacting ETAs.  

##  **Technologies Used**  
- **Streamlit**: For building the user interface.  
- **OpenCage API**: For geocoding.  
- **Geopy**: For geospatial distance calculation.  
- **scikit-learn**: For feature scaling and ML modeling.  
- **pandas/numpy**: For data preprocessing and transformation.  



##  **Repository Structure**  
```plaintext
Food-Delivery-Time-Prediction-Model/
│
├──  Food Delivery Prediction.ipynb   # Notebook for exploratory data analysis & model building.
├──  Location_finder_api.ipynb        # Fetch coordinates using OpenCage API.
├──  main.py                          # Streamlit app script.
├──  train.csv                        # Dataset containing delivery data.
├──  model.pkl                        # Trained machine learning model (serialized).
├──  scaler.pkl                       # Pretrained StandardScaler for feature scaling.
├──  README.md                        # Detailed project documentation.
├──  LICENSE                          # Open-source license information.
├──  resources/                       # Additional assets (e.g., images, logos).
│
└──  requirements.txt                 # List of dependencies.

```  


##  **Future Enhancements**  
1. **Real-Time Data Feeds**:  
   Integrate live traffic and weather data to improve prediction accuracy.  

2. **Advanced Modeling**:  
   Experiment with Gradient Boosting algorithms like **XGBoost** or **CatBoost** for enhanced performance.  

3. **Cloud Deployment**:  
   Host the app on AWS or Heroku for real-world testing.  

4. **Feedback Loop**:  
   Allow users to provide feedback on predictions, improving model retraining.  

5. **Multi-Language Support**:  
   Add localization for wider adoption.  



**Contributions** are welcome and encouraged—feel free to improve, extend, or suggest **enhancements!**

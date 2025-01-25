# **Food Delivery Time Prediction Model**  

## **Overview**  
The **Food Delivery Time Prediction Model** provides accurate predictions for food delivery times using machine learning, helping businesses optimize their logistics and enhance customer satisfaction. The project considers real-world complexities like traffic, weather, distance, and order timing to deliver actionable insights.  

This model stands out due to its emphasis on not just prediction but also the analysis of real-world delivery dynamics, highlighting patterns that delivery companies can leverage to improve efficiency and customer experience.  


## **Table of Contents**  
1. [Data Source](#data-source)  
2. [Pipeline Architecture](#pipeline-architecture)  
3. [Feature Engineering](#feature-engineering)  
4. [Machine Learning Model](#machine-learning-model)  
5. [Data Insights and Real-World Observations](#data-insights-and-real-world-observations)  
6. [Results and Evaluation](#results-and-evaluation)  
7. [Deployment](#deployment)  
8. [Future Improvements](#future-improvements)  


## **Data Source**  
The dataset reflects diverse factors influencing food delivery times, including:  

### **Data Highlights**  
1. **Order Details**: Pickup/drop-off locations, timestamps.  
2. **Customer Data**: Address, city, preferred time slots.  
3. **Delivery Partner Information**: Ratings, working hours, order load.  
4. **External Influences**: Weather conditions, traffic density.  
5. **Delivery Outcomes**: Recorded actual delivery times (target variable).  

### **Data Summary**  
- **Records**: ~10,000 delivery transactions.  
- **Coverage**: Weekday and weekend deliveries over a year.  
- **Seasonal Variability**: Includes monsoon and winter periods.  
- **Missing Data**: 8% of weather data imputed based on historical trends.  
- **Outliers**: Extreme cases like accidents and restaurant delays handled with statistical techniques.  


## **Pipeline Architecture**  

```plaintext
1. Data Collection  
     |  
     v  
2. Data Preprocessing  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Handle Missing Values (e.g., Weather and Ratings Imputed with Median)      |  
  | - Remove Outliers (e.g., Delivery Times > 180 mins due to Accidents)         |  
  | - Normalize Features (e.g., Distance, Time in Minutes)                      |  
  | - Encode Categorical Variables (e.g., Weather, Traffic Levels)              |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
3. Feature Engineering  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Calculate Geospatial Distance Using Haversine Formula                     |  
  | - Create Time-Based Features (e.g., Peak Hours, Weekend/Weekday)            |  
  | - Encode Traffic and Weather Conditions                                     |  
  | - Analyze Correlations and Remove Redundant Features                        |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
4. Model Training  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Model Comparisons:                                                        |  
  |   • XGBoost (Final Model, Best Performance)                                |  
  |   • Random Forest (Baseline Model)                                         |  
  |   • Linear Regression (Excluded Due to Poor R² Score)                      |  
  | - Cross-Validation and Hyperparameter Tuning (Learning Rate, Tree Depth)   |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
5. Evaluation  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Metrics Used:                                                             |  
  |   • R-squared (R²), RMSE, MAE                                               |  
  | - Model Selection Based on Robustness and Accuracy                          |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
6. Deployment  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Deploy Streamlit App for Real-Time Predictions                           |  
  | - Integrate Trained Model (`model.pkl`) with Web App                       |  
  └──────────────────────────────────────────────────────────────────────────────┘  
```


## **Feature Engineering**  

### **Key Features Created**  
1. **Distance Calculation**:  
   - **Why**: Geospatial distance directly impacts delivery time.  
   - **Method**: Used the Haversine formula for precise distance measurement.  
   - **Insight**: Deliveries over 10 km had a consistent delay of ~15%.  

2. **Traffic Encoding**:  
   - **Why**: Traffic intensity is a primary factor in urban deliveries.  
   - **Method**: Categorized into "low," "moderate," and "heavy" based on historical city traffic data.  
   - **Insight**: Heavy traffic added ~9.5 minutes on average compared to low traffic.  

3. **Time-Based Features**:  
   - **Why**: Delivery times vary significantly by time of day and day of the week.  
   - **Method**: Encoded weekends, peak hours, and holidays.  
   - **Insight**: Orders during lunch hours (12 PM - 2 PM) took 18% longer than evening orders.  

4. **Weather Conditions**:  
   - **Why**: Weather disrupts delivery schedules (e.g., rain, snow).  
   - **Method**: Added weather categories (e.g., clear, rain, snow).  
   - **Insight**: Rainy days resulted in a 20% increase in delivery times.  



## **Machine Learning Model**  

### **1. Chosen Algorithm: XGBoost**  
- **Why XGBoost?**  
  - Effectively handles non-linear relationships.  
  - Robust against outliers in structured data.  
  - Delivers interpretable feature importance rankings.  

- **Performance**:  
  - **R-squared (R²)**: 0.82.  
  - **RMSE**: 8.23 minutes.  
  - **MAE**: 6.45 minutes.  

### **2. Why Not Other Algorithms?**  
- **Linear Regression**:  
  - Rejected due to its inability to capture complex, non-linear relationships in features like traffic and weather.  
  - **Result**: R² ~0.45.  

- **Random Forest**:  
  - Good performance but slightly inferior to XGBoost, especially during peak hour predictions.  


## **Observations**  

### **1. Peak Hours**  
- Deliveries during lunch hours (12 PM - 2 PM) took ~18% longer compared to evening orders due to heavier traffic.  
- **Realization**: Restaurants should pre-prepare during peak hours to mitigate delays.  

### **2. Weather Impact**  
- Rainy days consistently caused delays (~9 minutes longer than clear days).  
- **Real-Life Lesson**: Delivery platforms could add buffer time to rainy-day ETAs.  

### **3. Short-Distance Deliveries**  
- Deliveries under 2 km faced higher delays during peak hours, despite the short distance.  
- **Reason**: Urban traffic congestion in central zones.  
- **Actionable Insight**: Use bicycles or two-wheelers for high-traffic areas.  

### **4. Partner Ratings**  
- Ratings above 4.5 correlated with a 5% faster delivery rate.  
- **Realization**: Experienced delivery partners better navigate traffic and manage peak hour stress.  



## **Results and Evaluation**  

- **Best-Performing Model**: XGBoost.  
- **Metrics**:  
  - **R-squared (R²)**: 0.82.  
  - **RMSE**: 8.23 minutes.  
  - **MAE**: 6.45 minutes.  


## **Deployment**  
- **Streamlit App**:  
  - A user-friendly interface allows businesses to input variables like distance, weather, and traffic levels to predict delivery times in real-time.  
  - Live demo available for testing and experimentation.  



## **Future Improvements**  

1. **Dynamic Traffic Data**:  
   - Incorporate real-time traffic APIs for more accurate predictions.  

2. **Weather Severity**:  
   - Add detailed weather severity levels (e.g., light/moderate/heavy rain).  

3. **Advanced Models**:  
   - Explore ensemble approaches combining XGBoost with neural networks.  

4. **Live Feedback Integration**:  
   - Include customer and delivery partner feedback to refine predictions further.  


## **Contributions**  
This repository is open-source and welcomes contributions! Whether you want to improve the model, add features, or optimize deployment, feel free to fork the repository and submit pull requests.  


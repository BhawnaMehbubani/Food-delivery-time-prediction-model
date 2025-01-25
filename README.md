# **Food Delivery Time Prediction Model**  

## **Overview**  
This project delivers a cutting-edge **food delivery time prediction model** designed to enhance delivery logistics and optimize customer satisfaction. By leveraging state-of-the-art machine learning algorithms, advanced geospatial analysis, and a user-friendly deployment interface, it offers both precise predictions and actionable insights.  

**Tech Stack Highlights**:  
- **Machine Learning**: Random Forest Regressor, XGBoost for predictive modeling.  
- **Feature Engineering**: Advanced techniques for geospatial and temporal features.  
- **API Integration**: OpenCage API and GeoPy for precise location mapping.  
- **Frontend**: Streamlit for seamless real-time prediction and interactive visualization.  


## **Table of Contents**  
1. [Data Source](#data-source)  
2. [Pipeline Architecture](#pipeline-architecture)  
3. [Feature Engineering](#feature-engineering)  
4. [Machine Learning Model](#machine-learning-model)  
5. [Technologies and Tools](#technologies-and-tools)  
6. [Data Insights and Real-World Observations](#data-insights-and-real-world-observations)  
7. [Results and Evaluation](#results-and-evaluation)  
8. [Deployment](#deployment)  
9. [Future Improvements](#future-improvements)  


## **Data Source**  
The dataset encapsulates various real-world attributes influencing delivery times:  

- **Order Details**: Customer locations, pickup/drop-off points.  
- **Delivery Partner Metrics**: Ratings, historical performance, current workload.  
- **External Factors**: Weather conditions, traffic levels, peak hours.  
- **Delivery Outcomes**: Actual delivery times (target variable).  


## **Pipeline Architecture**  

```plaintext
Data Collection  
     |  
     v  
Data Preprocessing  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Missing Value Handling (Weather, Ratings via Median Imputation)           |  
  | - Outlier Removal (Extreme Delivery Times > 180 mins due to Accidents)      |  
  | - Standardizing Numerical Features (Distance, Time)                        |  
  | - Encoding Categorical Variables (Traffic Levels, Weather Conditions)      |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
Feature Engineering  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Geospatial Distance Calculation Using GeoPy and Haversine Formula         |  
  | - Time-Based Features (Weekends, Peak Hours)                                |  
  | - API Integration for Address Normalization Using OpenCage API              |  
  | - Correlation Analysis to Select Optimal Features                           |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
Model Selection and Training  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Compared Algorithms:                                                     |  
  |   • XGBoost (Chosen for Accuracy, Feature Insights)                        |  
  |   • Random Forest (Robust Baseline Model)                                  |  
  |   • Linear Regression (Excluded for Poor Fit on Non-linear Data)           |  
  | - Hyperparameter Tuning via Grid Search                                    |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
Model Evaluation  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Metrics Used:                                                            |  
  |   • R² Score: Goodness of Fit                                              |  
  |   • RMSE: Overall Prediction Accuracy                                      |  
  |   • MAE: Average Deviation in Predictions                                  |  
  └──────────────────────────────────────────────────────────────────────────────┘  
     |  
     v  
Deployment  
  ┌──────────────────────────────────────────────────────────────────────────────┐  
  | - Built with Streamlit for Real-Time Predictions and Interactive Interface  |  
  | - Model Integration (`model.pkl`)                                          |  
  └──────────────────────────────────────────────────────────────────────────────┘  
```


## **Feature Engineering**  

### **Key Features**  

1. **Distance Calculation**:  
   - **Tools Used**: GeoPy, Haversine Formula, OpenCage API for geocoding.  
   - **Insight**: Deliveries over 15 km showed higher variability due to inconsistent traffic patterns.  

2. **Time-Based Features**:  
   - **What**: Weekends, peak hours, and holidays significantly impact delivery times.  
   - **Insight**: Weekend orders were 20% faster due to lower traffic levels.  

3. **Traffic and Weather Conditions**:  
   - **Tools Used**: Encoded categories for "Clear," "Rain," and "Heavy Traffic."  
   - **Insight**: Rainy weather consistently added 9 minutes to delivery times, and heavy traffic extended it by ~12 minutes.  

4. **Delivery Partner Ratings**:  
   - **What**: Correlated partner ratings with delivery efficiency.  
   - **Insight**: Partners rated >4.5 completed deliveries 7% faster on average.  


## **Machine Learning Model**  

### **Algorithm Selection**  
1. **XGBoost (Final Model)**:  
   - Outperformed others due to its ability to handle complex, non-linear relationships.  
   - Provided interpretable feature importance scores.  

2. **Random Forest (Baseline)**:  
   - Delivered strong baseline performance but lacked the precision of XGBoost in edge cases.  

3. **Linear Regression**:  
   - Rejected due to poor performance (R² ~0.45) on non-linear features like weather and traffic.  


## **Technologies and Tools**  

| **Technology** | **Purpose** |  
|-----------------|-------------|  
| **Python**     | Core programming language for modeling and analysis. |  
| **Jupyter**    | Environment for exploratory data analysis and prototyping. |  
| **Streamlit**  | Web app for deployment and real-time prediction. |  
| **GeoPy**      | Calculated geospatial distances. |  
| **OpenCage API** | Geocoded addresses to standardize location data. |  
| **XGBoost**    | Machine learning model with the highest accuracy. |  
| **Random Forest** | Baseline model for robustness comparisons. |  



## **Data Insights and Real-World Observations**  

1. **Peak Hours Delay**:  
   - Lunch-time deliveries faced 18% longer delays due to high traffic.  
   - **Realization**: Restaurants can pre-prepare meals for peak orders.  

2. **Weather Disruptions**:  
   - Rainy days increased delivery times by ~20%, with heavy rain causing delays up to 30%.  
   - **Actionable Insight**: Incorporate weather forecasts into ETA calculations.  

3. **Short-Distance Traffic Bottlenecks**:  
   - Urban congestion caused delays for deliveries under 2 km, especially in city centers.  
   - **Recommendation**: Use bikes or two-wheelers for dense urban areas.  

4. **Delivery Partner Efficiency**:  
   - High-rated partners consistently outperformed their peers in timely deliveries.  
   - **Lesson**: Incentivize partners with higher ratings during peak periods.  


## **Results and Evaluation**  

- **Best Model**: XGBoost  
- **Performance Metrics**:  
  - **R² Score**: 0.82  
  - **RMSE**: 8.23 minutes  
  - **MAE**: 6.45 minutes  


## **Deployment**  

**Streamlit Application**:  
- Deployed a real-time interface for predictions.  
- Allows users to input features (distance, weather, traffic) and instantly receive delivery time estimates.  


## **Future Improvements**  

1. **Real-Time Traffic API**: Integrate live traffic data for dynamic predictions.  
2. **Weather Severity Analysis**: Include detailed weather data like storm warnings.  
3. **Feature Expansion**: Add features like restaurant preparation times.  
4. **Advanced Models**: Experiment with ensemble models combining XGBoost and neural networks.  


## **Contributions**  
Contributions are welcome! Feel free to fork the repository, make improvements, and submit a pull request.  


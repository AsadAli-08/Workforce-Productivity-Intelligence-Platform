# Workforce-Productivity-Intelligence-Platform

## Project Overview:
* Organizations today generate large volumes of workforce and operational data, but much of this information is often used only for basic reporting and historical analysis. This project was developed to explore how workforce data can be transformed into actionable business intelligence that supports productivity improvement, workforce planning, and operational decision-making.

* The Workforce Productivity Intelligence Platform is an end-to-end analytics project built using Oracle SQL, Python, and Power BI. The project combines workforce analytics, machine learning, and executive dashboarding to analyze workforce productivity, identify operational performance patterns, and predict productivity risk across the organization.

* The solution uses workforce indicators such as engagement, overtime, attendance, career progression, learning participation, and compensation trends to understand the factors influencing workforce productivity and long-term workforce sustainability.

* A Logistic Regression machine learning model was developed to predict productivity deterioration risk and identify the major workforce conditions contributing to that risk. The project also includes employee-level driver attribution to improve explainability and support operational decision-making.

* The Power BI dashboards were designed using a modern enterprise-style approach inspired by executive workforce intelligence platforms. The dashboards provide visibility into workforce productivity, operational performance, workforce risk drivers, intervention priorities, and workforce capability readiness.

Overall, the project aims to show how organizations can move beyond traditional HR reporting and use workforce intelligence to support more proactive and strategic decision-making.


## Objectives:
The goal of this project is to build an end-to-end workforce productivity intelligence platform that helps organizations better understand workforce performance, identify productivity risks, and support data-driven workforce decisions.

The project focuses on combining workforce analytics, machine learning, and interactive dashboards to move beyond traditional HR reporting and provide deeper operational and strategic insights.

The main objectives of the project are:

* Analyze workforce productivity across different business units, grades, and workforce segments
* Identify workforce patterns and operational factors influencing productivity performance
* Predict productivity deterioration risk using machine learning techniques
* Understand the key drivers contributing to workforce productivity risk
* Build explainable and interpretable workforce risk models using Logistic Regression
* Develop executive dashboards for workforce monitoring and operational decision-making
* Identify workforce hotspots requiring intervention and performance improvement
* Analyze workforce capability, learning participation, and career progression trends
* Measure workforce readiness and long-term capability sustainability
* Create a modern enterprise-style workforce intelligence solution using SQL, Python, and Power BI

## Tools & Technologies:

* SAP HCM: Primary workforce data source
* Oracle DB: Enterprise data storage layer
* SQL: Data transformation, KPI calculation, Feature engineering, and Analytical data preparation
* Power BI: Data modeling, Business intelligence, Dashboard development, and Visualization
* Python/ Scikit Learn: Machine learning, Predictive analytics, and Workforce risk modeling
* GitHub: Version control, Project management, and Technical documentation

## Project Architecture:


* Workforce Data Collection & Storage
        
  ↓  
        
* Oracle SQL Data Preparation & Transformation
        
  ↓  
        
* Feature Engineering & Workforce KPI Development
        
  ↓  
        
* Python-Based Machine Learning & Risk Prediction
        
  ↓  
        
* Power BI Data Modeling & Analytics
        
  ↓  
        
* Workforce Productivity Intelligence Dashboards


## Data Model:

        Fact Tables
        
        ↓
                
        Feature Engineering Tables
        
        ↓
                
        Workforce Monthly Snapshot
        
        ↓
                
        Python Machine Learning & Risk Prediction
        
        ↓
                
        Productivity Risk Probability & Driver Attribution


## Data Tables/ Data Fields/ Metrics/ Features:

### Fact Tables:

* FACT_EMP_MASTER (Employee Master)
  * EMP_ID (Employee ID)
  * GENDER
  * DOB
  * DOJ
  * DOR
 
* FACT_CAREER (Career Progression of Employees over the years)
  * EMP_ID
  * FROM_DT
  * TO_DT
  * UNIT
  * LOCATION
  * CADRE
  * GRADE
  * DEPTT
  * POSITION
    
* FACT_EMP_SNAP_MONTH (Monthly Snapshot of Employee Master data)
  * SNAPSHOT_MONTH
  * EMP_ID
  * DEPTT
  * TENURE_YY
  * AGE_YY
  * CADRE
  * GRADE
  * POSITION
  * GENDER
  * DOJ
  * DOR
  * UNIT
  * LOCATION
    
* FACT_ATTENDANCE (Monthly attendance details of employees)
  * EMP_ID
  * KEY_DATE
  * WORKING_DAYS
  * ABSENCE_DAYS
  * SICK_LEAVES
  * CASUAL_LEAVES
  * LATE_LOGINS
  * EARLY_EXITS
  * OVERTIME_HH

* FACT_COMPENSATION (Monthly compensation details of employees)
  * EMP_ID
  * KEY_DATE
  * BASIC
  * BONUS
  * INCENTIVE
  * TOTAL SALARY
  * COMPA_RATIO (Employee Salary / Salary Range Midpoint)
  * SAL_GROWTH (Annual)
    
* FACT_ENGAGEMENT (Results of Quarterly Engagement Survey)
  * EMP_ID
  * FROM_DT
  * TO_DT
  * ENGAGEMENT_SCORE (Overall employee engagement level)
  * BURNOUT_SCORE (Employee burnout or exhaustion level)
  * WELLBEING_SCORE (Employee physical/mental wellbeing indicator)
  * MANAGER_FEEDBACK_SCORE (Employee Perception of Manager)
  * CULTURE_SCORE (Employee perception of organizational culture)
  * WORKLOAD_SCORE (Employee perception of workload)

* FACT_PERFORMANCE
  * EMP_ID
  * FROM_DT
  * TO_DT
  * RATING (Final employee appraisal rating out of 5)
  * KPI_SCORE (Actual KPI / Target KPI) × 100
  * GOAL_PERC (Percentage of goals completed)
  * MGR_RATING (Manager-assigned evaluation score out of 5)
  * POTENTIAL (Future growth/leadership potential assessment score out of 5)
  * PERF_CHANGE (Difference between current and previous rating)
    
* FACT_PRODUCTIVITY (Monthly Productivity details of employees)
  * EMP_ID
  * KEY_DATE
  * TASKS_ASSIGNED (Number of tasks allocated to employee)
  * TASKS_COMPLETED (Number of tasks successfully completed)
  * PROD_RATE (TASKS_COMPLETED / TASKS_ASSIGNED) * 100
  * TIMELINESS (On-Time Tasks / TASKS_COMPLETED) * 100
  * ERROR_RATE (Errored Tasks / TASKS_COMPLETED) * 100
  * WORK_HOURS
  * PRODUC_HOURS (Actual productive work hours) 
  * UTILISE_RATE (PRODUCTIVE_HOURS / AVAILABLE_WORK_HOURS) * 100
      
* FACT_PROMOTION (Employee Promotion Details)
  * EMP_ID
  * PROMOTION_DATE
  
* FACT_TRAINING  (Employee Training Details)
  * EMP_ID
  * KEY_DATE
  * TRAINING_ID
  * HOURS (Total learning duration completed)
  * SCORE (Assessment/evaluation score achieved in training)
  * COMPLETED (Indicates whether certification was successfully completed)
    
### Dimension Tables:
* DIM_MONTHS
  * SNAPSHOT_MONTH
    
* DIM_UNIT
  * Unit_Code
  * Unit_Name
    
* DIM_LOCATION
  * Location_Code
  * Location_Name
    
* DIM_GRADE
  * Grade_Code
  * Grade_Name
  * Cadre_Name
 
* DIM_FUNCTION
  * FUNCTION_CODE
  * Function_Name 

### Feature Engineering Tables:
* FEAT_ATTENDANCE
  * SNAPSHOT_MONTH
  * EMP_ID
  * OVERTIME_3M_AVG (Rolling overtime 3 month average)
  * ABSENCE_3M_AVG (Rolling absence 3 month average)
  * CONS_OVERTIME_MONTHS (Count of consecutive high-overtime months (>20 Hours per month))
  * LEAVE_SPIKE_FLAG (If 3 month average absence * 1.5 < Absence in current month --> Then 1 Else 0)
    
* FEAT_PRODUCTIVITY
  * SNAPSHOT_MONTH
  * EMP_ID
  * PROD_3M_AVG (Rolling 3-month productivity rate average)
  * PROD_3M_SLOPE (Current prod rate – prod rate 2 months back)/ 3
  * PROD_VOLATILITY (STDDEV of Productivity Rate over 6 months)
    
* FEAT_PROMOTION
  * SNAPSHOT_MONTH
  * EMP_ID
  * STAGFLATION (Is years since last promotion > 4 Then 1 Else 0)
  * PROM_COUNT (Number of Promotions Till Date)
  * YY_SINCE_PROM (Years Since Last Promotion)
    
* FEAT_TRAINING
  * SNAPSHOT_MONTH
  * EMP_ID
  * TRAIN_HH_6M (Sum of training hours in last 6 months)
  * AVG_TRAIN_SCORE_6M (Average training assessment score)
  * CERT_COUNT (Count of completed certifications)
    
* FEAT_MONTHLY_SNAPSHOT
  * SNAPSHOT_MONTH
  * EMP_ID
  * WORKING_DAYS
  * ABSENCE_DAYS
  * SICK_LEAVES
  * CASUAL_LEAVES
  * LATE_LOGINS
  * EARLY_EXITS
  * OVERTIME_HH
  * TRAIN_HH_6M
  * AVG_TRAIN_SCORE_6M
  * CERT_COUNT
  * PROD_3M_AVG
  * PROD_3M_SLOPE
  * PROD_VOLATILITY
  * PROD_RISK_FLAG {IF PRODUCTIVITY_RATE < 70 OR ( PROD_3M_SLOPE < 0 AND PRODUCTIVITY_VOLATILITY > 15 ) OR ERROR_RATE > 10 THEN 1 ELSE 0}
  * OVERTIME_3M_AVG
  * ABSENCE_3M_AVG
  * CONS_OVERTIME_MONTHS
  * LEAVE_SPIKE_FLAG
  * PAY_GROWTH_TREND
  * COMP_STAG_FLAG
  * GENDER
  * TENURE_YY
  * AGE_YY
  * CADRE
  * GRADE
  * UNIT
  * LOCATION
  * DOR
  * YY_SINCE_PROM
  * STAGFLATION
  * CAREER_VELOCITY
  * TASKS_ASSIGNED
  * TASKS_COMPLETED
  * PROD_RATE
  * TIMELINESS
  * ERROR_RATE
  * WORK_HOURS
  * UTILISE_RATE
  * PRODUC_HOURS
  * HIGH_PROD_FLAG
  * RATING
  * KPI
  * GOAL_PERC
  * MGR_RATING
  * POTENTIAL
  * PERF_CHANGE
  * BASIC
  * BONUS
  * INCENTIVE
  * DOJ
  * COMPA_RATIO
  * SAL_GROWTH
  * ENGAGE_SCORE
  * BURN_SCORE
  * WELL_SCORE
  * TOTAL
  * CULTURE_SCORE
  * WORKLOAD_SCORE
  * MANAGER_SCORE
  
* FEAT_PROD_RISK_PROBAB (Latest Snapshot with Productivity Risk Probability & Future Readiness Index as generated using Python)
  * SNAPSHOT_MONTH
  * EMP_ID
  * GENDER
  * TENURE_YY
  * AGE_YY
  * CADRE
  * GRADE
  * UNIT
  * LOCATION
  * DOJ
  * DOR
  * YY_SINCE_PROM
  * STAGFLATION
  * CAREER_VELOCITY
  * TASKS_ASSIGNED
  * TASKS_COMPLETED
  * PROD_RATE
  * TIMELINESS
  * ERROR_RATE
  * WORK_HOURS
  * UTILISE_RATE
  * PRODUC_HOURS
  * HIGH_PROD_FLAG
  * RATING
  * KPI
  * GOAL_PERC
  * MGR_RATING
  * POTENTIAL
  * PERF_CHANGE
  * BASIC
  * BONUS
  * INCENTIVE
  * TOTAL
  * COMPA_RATIO
  * SAL_GROWTH
  * ENGAGE_SCORE
  * BURN_SCORE
  * WELL_SCORE
  * MANAGER_SCORE
  * CULTURE_SCORE
  * WORKLOAD_SCORE
  * WORKING_DAYS
  * ABSENCE_DAYS
  * SICK_LEAVES
  * CASUAL_LEAVES
  * LATE_LOGINS
  * EARLY_EXITS
  * OVERTIME_HH
  * TRAIN_HH_6M
  * AVG_TRAIN_SCORE_6M
  * CERT_COUNT
  * PROD_3M_AVG
  * PROD_3M_SLOPE
  * PROD_VOLATILITY
  * PROD_RISK_FLAG
  * OVERTIME_3M_AVG
  * ABSENCE_3M_AVG
  * CONS_OVERTIME_MONTHS
  * LEAVE_SPIKE_FLAG
  * BURNOUT_RISK_FLAG
  * PAY_GROWTH_TREND
  * COMP_STAG_FLAG
  * PROD_RISK_PROB
  * READINESS_INDEX

* FEAT_PROD_RISK_FACTORS (Coefficients of the Logistic Regression Model as generated using ML)
  * FEATURE
  * COEFFICIENT
   
## Machine Learning Model:
The primary objective of the model is to:

* predict the probability of future productivity deterioration,
* identify workforce conditions contributing to productivity risk,
* support proactive workforce intervention and decision-making.

Instead of focusing only on current productivity levels, the model was designed to identify patterns that may indicate future workforce performance decline.

The model used target variable PROD_RISK_FLAG which represents employees showing elevated productivity deterioration risk based on operational productivity indicators such as:
* productivity decline trend
* productivity instability
* low productivity levels
* quality deterioration
* operational inefficiency signals

The project uses a Logistic Regression model. Logistic Regression was selected because it provides:
* strong interpretability
* transparent prediction logic
* explainable feature contribution analysis
* suitability for workforce analytics and business environments

The model generates:
* productivity risk probability scores
* employee-level workforce risk classification
* top drivers contributing to productivity risk

## Power BI Dashboard:
### Executive Overview
* Filters: Date/ Unit/ Location/ Cadre/ Grade
* Buttons: Data Reset Button
* Metrics: Avg Productivity Rate/ Avg Risk Probability/ Avg Timeliness/ Future Ready %
* Visuals: Producitivity Rate Trend Analysis(Line Chart)/ Top Productivity Risk Drivers(Column Chart) / Top 10 Employees at Risk(Table) / Future Readiness Segmentation(Bar Chart) 
  
### Workforce Productivity Intelligence
* Filters: Date/ Unit/ Location/ Cadre/ Grade
* Buttons: Data Reset Button
* Metrics: Avg Productivity Rate/ High Performer %/ High Risk Employee Count/ Avg Utilisation Rate
* Visuals: Producitivity Rate Trend Analysis(Line Chart)/ Productivity Distribution Analysis(Histogram)/ Productivity Ranking by Unit(Bar Chart)/ Productivity Decomposition Tree (Decomposition Tree)
* Observations:
  
        * Workforce productivity remained largely stable over the analysis period, fluctuating marginally around the 86% mark.
       
        * Nearly 96% of employees fall within the High or Very High productivity bands, indicating a strong overall productivity profile.
  
        * EPD recorded the highest productivity levels among business units, while PC showed comparatively lower productivity performance.
  
        * Productivity performance improves across higher workforce grades, with E11 recording the highest average productivity.
  
        * Significant location-level variation exists, with Gorakhpur showing the highest productivity and Lakwa recording the lowest productivity levels.

  
### Productivity Risk Driver Lab
* Filters: Unit/ Location/ Cadre/ Grade/ Risk Band
* Buttons: Data Reset Button
* Metrics: High Risk Employee Count/ Avg Risk Probability/ Top Risk Driver/ Burnout Risk Correlation/ Burnout Risk % 
* Visuals: Machine Learning Driver Importance (Bar Chart)/ Top Productivity Risk Drivers(Column Chart)/ Top Risk Driver v/s Productivity Risk(Scatter Plot)/ Productivity Risk Heatmap (Matrix)
* Observations:
  
        * Productivity risk increases with higher overtime, lower engagement, weaker KPI performance, low certifications, poor manager ratings, and compensation stagnation.
  
        * Strong manager support, longer tenure, stable recent productivity trends, and lower leave spikes are linked with lower productivity risk.

        * Productivity risk is most strongly influenced by recent productivity trends, with PROD_3M_SLOPE emerging as the most significant driver in the model, followed by tenure, manager rating, and KPI performance.
  
  
### Workforce Action Centre
* Filters: Unit/ Location/ Cadre/ Grade/ Risk Band
* Buttons: Data Reset Button
* Metrics: High Risk Employee Count/ High Risk Employee %/ Avg Productivity Risk Probability
* Visuals: Employee Productivity Risk Register(Table)/ High Risk Units(Bar Chart)/ Workforce Risk Quadrant(Scatter Plot)
* Observations:

        * CFFP shows the highest average productivity risk (43%), while EPD records the lowest risk levels (10%).
  
        * Risk levels are highest in junior grades, with E1 employees showing the highest risk exposure, while E11 employees show the lowest risk levels.
  
        * The E9 grade within the ROD unit emerges as the most critical hotspot, with productivity risk reaching nearly 80%.
  
### Growth & Capability Intelligence
* Filters: Unit/ Location/ Cadre/ Grade/ Risk Band
* Buttons: Data Reset Button
* Metrics: Future Ready %/ Certified %/ Avg Career Velocity/ Avg Training Hours/ Compensation Stagnation %
* Visuals: Learning Effectiveness Analysis(Line & Column Chart)/ Career Velocity Distribution(Column Chart)/ Workforce Capability Matrix(Scatter Plot)/ Future Readiness Segmentation (Funnel Chart)
* Observations:

          * Employee engagement improves steadily with training participation up to nearly 85 training hours, after which engagement levels begin to decline, indicating possible learning saturation or training fatigue.
  
          * Nearly 73% of employees fall within the Rapid Career Velocity segment, while only 5% show signs of career stagnation and 1% fall into the critical career risk category.
  
          * Productivity levels show a positive relationship with training hours, suggesting that workforce learning and capability development contribute positively to operational performance.
  
          * Almost half of the workforce (47%) falls within the Growth Potential segment, while development support is still required for 38% of employees.
  
          * Only 8% of employees currently qualify as Future Ready, while another 8% are classified under Career Stagnation Risk, highlighting opportunities for long-term workforce capability improvement.

## Key Insights:

* Workforce productivity remained stable throughout the analysis period, with average productivity consistently maintained around the 86% level.
* Nearly 96% of employees fall within the High or Very High productivity categories, reflecting a strong overall workforce performance profile.
* Significant productivity variation exists across business units, grades, and locations, indicating differences in operational efficiency and workforce performance across the organization.
* Machine learning analysis identified recent productivity trends (PROD_3M_SLOPE) as the strongest predictor of future productivity deterioration risk.
* Higher overtime, low engagement, weaker KPI performance, poor manager ratings, low certifications, and compensation stagnation significantly increase productivity risk probability.
* Strong managerial support, longer employee tenure, and stable recent productivity trends are associated with lower workforce productivity risk.
* CFFP emerged as the highest-risk business unit, while EPD recorded the lowest overall productivity risk levels.
* Productivity risk is concentrated within junior workforce grades, with the E9 grade in the ROD unit identified as the most critical workforce risk hotspot.
* Workforce learning and development show a positive relationship with productivity performance, highlighting the impact of capability-building initiatives on operational outcomes.
* While a large portion of employees demonstrate strong career growth potential, only a limited percentage currently qualify as Future Ready, indicating opportunities to strengthen long-term workforce capability and talent readiness.

## Recommendations:

* Prioritize intervention strategies for high-risk workforce segments, particularly junior grades and critical operational hotspots such as the E9 workforce segment within the ROD unit, where productivity risk levels are significantly elevated.
* Strengthen workforce engagement and managerial effectiveness initiatives, as low engagement, weaker KPI performance, and poor manager ratings emerged as major contributors to productivity deterioration risk.
* Monitor overtime and workload pressure more closely to reduce long-term workforce sustainability risks and prevent productivity decline caused by operational fatigue.
* Expand targeted learning and capability development programs, especially for employees classified under Development Required and Career Stagnation Risk segments, to improve long-term workforce readiness and productivity stability.
* Develop a structured workforce growth and retention strategy focused on career progression, certifications, and compensation progression to reduce compensation stagnation risk and strengthen future workforce capability.

## Author:

Asad Ali

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
    
* FACT_PRODUCTIVITY
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
      
* FACT_PROMOTION
* FACT_TRAINING
* FACT_EMP_SNAP_MONTH
* FACT_PROD_RISK_FACTORS
* FACT_PROD_RISK_PROBAB

### Dimension Tables:
* DIM_MONTHS
* DIM_UNIT
* DIM_LOCATION
* DIM_GRADE

### Feature Tables:
* PERF_FT_ATTENDANCE
* PERF_FT_PRODUCTIVITY
* PERF_FT_PROMOTION
* PERF_FT_TRAINING
* FACT_MONTHLY_SNAPSHOT

## Machine Learning Model:

## Power BI Dashboard:

## Key Insights:

## Recommendations:

## Author:

Asad Ali

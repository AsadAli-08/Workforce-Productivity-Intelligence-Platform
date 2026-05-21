# DAX : Data Analysis Expressions

## Workforce Producitivity Intelligence

* Productivity Band =
      IF (
          FACT_PROD_RISK_PROBAB[Prod Rate] >= 90,
          "Very High",
          IF (
              FACT_PROD_RISK_PROBAB[Prod Rate] >= 75,
              "High",
              IF ( FACT_PROD_RISK_PROBAB[Prod Rate] >= 60, "Moderate", "Low" )
          )
      )
  
* High Performer % =
          DIVIDE (
              CALCULATE (
                  COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] ),
                  FACT_PROD_RISK_PROBAB[Productivity Band] = "Very High"
                      || FACT_PROD_RISK_PROBAB[Productivity Band] = "High"
              ),
              COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] )
          )
      
* High Risk Employees =
          CALCULATE (
              COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] ),
              FACT_PROD_RISK_PROBAB[Risk Band] = "High"
          )





## Productivity Risk Driver Lab

## Workforce Action Centre

## Growth & Capability Intelligence

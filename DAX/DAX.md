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

 
* Top Risk Driver =
  
            CALCULATE (
                SELECTEDVALUE ( FACT_PROD_RISK_FACTORS[FEATURE] ),
                FACT_PROD_RISK_FACTORS[Abs Coeff] = MAX ( FACT_PROD_RISK_FACTORS[Abs Coeff] )
            )


* Burnout Risk Correlation = 
            VAR MeanX =
                AVERAGE('FACT_PROD_RISK_PROBAB'[Burn Score])
            
            VAR MeanY =
                AVERAGE('FACT_PROD_RISK_PROBAB'[Risk Probability])
            
            VAR Numerator =
                SUMX(
                    'FACT_PROD_RISK_PROBAB',
                    (
                        'FACT_PROD_RISK_PROBAB'[Burn Score] - MeanX
                    ) *
                    (
                        'FACT_PROD_RISK_PROBAB'[Risk Probability] - MeanY
                    )
                )
            
            VAR DenominatorX =
                SQRT(
                    SUMX(
                        'FACT_PROD_RISK_PROBAB',
                        POWER(
                            'FACT_PROD_RISK_PROBAB'[Burn Score] - MeanX,
                            2
                        )
                    )
                )
            
            VAR DenominatorY =
                SQRT(
                    SUMX(
                        'FACT_PROD_RISK_PROBAB',
                        POWER(
                            'FACT_PROD_RISK_PROBAB'[Risk Probability] - MeanY,
                            2
                        )
                    )
                )
            
            RETURN
            DIVIDE(
                Numerator,
                DenominatorX * DenominatorY
            )


* Burnout Risk % =
  
            DIVIDE(
                CALCULATE(
                    COUNTROWS('FACT_PROD_RISK_PROBAB'),
                    'FACT_PROD_RISK_PROBAB'[Burnout_Risk] = "High Risk"
                ),
                COUNTROWS('FACT_PROD_RISK_PROBAB')
            )



## Workforce Action Centre

* High Risk Employees = 

      CALCULATE (
          COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] ),
          FACT_PROD_RISK_PROBAB[Risk Band] = "High"
      )

* High Risk Employee % = 

      DIVIDE (
    CALCULATE (
        COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] ),
        FACT_PROD_RISK_PROBAB[Risk Band] = "High"
    ),
    COUNT ( FACT_PROD_RISK_PROBAB[Emp ID] )
)





## Growth & Capability Intelligence

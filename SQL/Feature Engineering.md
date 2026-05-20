# SQL

## DIM_MONTHS

       SELECT Add_months(Trunc(Add_months(SYSDATE, -60), 'MM'), LEVEL - 1) AS
              SNAPSHOT_MONTH
       FROM   dual
       CONNECT BY LEVEL <= Months_between( Trunc(SYSDATE, 'MM'), Trunc(Add_months(
                           SYSDATE, -60),
                           'MM') )
                           + 1 

## FACT_EMP_MASTER

       SELECT emp_id,
              emp_sex                     AS gender,
              emp_dateofbirth             AS dob,
              emp_bhel_jn_date            AS doj,
              emp_defunct                 AS status,
              emp_date_nrml_retirement_n0 AS dor
       FROM   saphr.pis_employee_master
       WHERE  ( Months_between(Trunc(SYSDATE), emp_date_nrml_retirement_n0) <= 60
                 OR emp_date_nrml_retirement_n0 > Trunc(SYSDATE) )
              AND emp_curr_eg = 1
              AND emp_dateofbirth < emp_bhel_jn_date 

## EMP_SNAP_MONTH

       SELECT m.snapshot_month,
              e.emp_id,
              e.gender,
              e.doj,
              e.dor,
              c.unit,
              c.location,
              c.cadre,
              c.grade,
              c.position,
              c.deptt,
              Round(Months_between(Last_day(m.snapshot_month), e.doj) / 12, 2) AS
              tenure_yy,
              Trunc(Months_between(Last_day(m.snapshot_month), e.dob) / 12)    AS
              age_yy
       FROM   perf_dim_months m
              left join perf_fact_emp_master e
                     ON e.doj <= Last_day(m.snapshot_month)
                        AND ( e.dor > Last_day(m.snapshot_month)
                               OR e.dor IS NULL )
              left join perf_fact_career c
                     ON c.emp_id = e.emp_id
                        AND c.from_dt <= Last_day(m.snapshot_month)
                        AND c.to_dt >= Last_day(m.snapshot_month)

## FEAT_ATTENDANCE

       SELECT m.snapshot_month,
              m.emp_id,
              /* 3-month overtime average */
              Round(Avg(t.overtime_hh)
                      over (
                        PARTITION BY t.emp_id
                        ORDER BY t.key_date ROWS BETWEEN 2 preceding AND CURRENT ROW ),
              2) AS
              overtime_3m_avg,
              /* 3-month absence average */
              Round(Avg(t.absence_days)
                      over (
                        PARTITION BY t.emp_id
                        ORDER BY t.key_date ROWS BETWEEN 2 preceding AND CURRENT ROW ),
              2) AS
              absence_3m_avg,
              /* Consecutive overtime months */
              CASE
                WHEN t.overtime_hh > 20
                     AND Lag(t.overtime_hh, 1)
                           over (
                             PARTITION BY t.emp_id
                             ORDER BY t.key_date ) > 20
                     AND Lag(t.overtime_hh, 2)
                           over (
                             PARTITION BY t.emp_id
                             ORDER BY t.key_date ) > 20 THEN 3
                WHEN t.overtime_hh > 20
                     AND Lag(t.overtime_hh, 1)
                           over (
                             PARTITION BY t.emp_id
                             ORDER BY t.key_date ) > 20 THEN 2
                WHEN t.overtime_hh > 20 THEN 1
                ELSE 0
              END
              AS cons_overtime_months,
              /* Leave spike detection */
              CASE
                WHEN t.absence_days > ( Avg(t.absence_days)
                                          over (
                                            PARTITION BY t.emp_id
                                            ORDER BY t.key_date ROWS BETWEEN 3
                                          preceding AND 1
                                          preceding ) * 1.5 ) THEN 1
                ELSE 0
              END
              AS leave_spike_flag
       FROM   perf_emp_snap_month m
              left join perf_fact_attendance t
                     ON m.emp_id = t.emp_id
                        AND m.snapshot_month = t.key_date

## FEAT_COMPENSATION

       SELECT m.snapshot_month,
              m.emp_id,
              Round(Avg(c.sal_growth)
                      over (
                        PARTITION BY c.emp_id
                        ORDER BY c.key_date ROWS BETWEEN 5 preceding AND CURRENT ROW ),
              2) AS
              PAY_GROWTH_TREND,
              ( CASE
                  WHEN Max(c.sal_growth)
                         over (
                           PARTITION BY c.emp_id
                           ORDER BY c.key_date ROWS BETWEEN 11 preceding AND CURRENT
                         ROW ) < 3
                THEN 1
                  ELSE 0
                END )
              AS COMP_STAG_FLAG
       FROM   perf_emp_snap_month m
              left outer join perf_fact_compensation c
                           ON m.emp_id = c.emp_id
                              AND m.snapshot_month = c.key_date 

## FEAT_PRODUCTIVITY

       SELECT
           m.snapshot_month,
           m.emp_id,
       
           /* 3-month rolling average */
           round(AVG(p.prod_rate) OVER (
               PARTITION BY p.emp_id
               ORDER BY p.key_date
               ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
           ),2) AS prod_3m_avg,
       
           /* Approximate 3-month trend */
           round((
               p.prod_rate -
               LAG(p.prod_rate, 2) OVER (
                   PARTITION BY p.emp_id
                   ORDER BY p.key_date
               )
           ) / 3,2) AS prod_3m_slope,
       
           /* 6-month volatility */
           round(STDDEV(p.prod_rate) OVER (
               PARTITION BY p.emp_id
               ORDER BY p.key_date
               ROWS BETWEEN 5 PRECEDING AND CURRENT ROW
           ),2) AS prod_volatility
       
       FROM perf_emp_snap_month m
       
       LEFT JOIN perf_fact_productivity p
       ON m.emp_id = p.emp_id
       AND m.snapshot_month = p.key_date

## FEAT_PROMOTION

       SELECT m.snapshot_month,
              m.emp_id,
              Round(Months_between(Last_day(m.snapshot_month), Max(p.prom_date)) / 12,
              2) AS
              yy_since_prom,
              CASE
                WHEN Round(Months_between(Last_day(m.snapshot_month), Max(p.prom_date))
                           / 12,
                     2) > 4 THEN 1
                ELSE 0
              END
              AS stagflation,
              Count(p.emp_id)
              AS prom_count
       FROM   perf_emp_snap_month m
              left outer join perf_fact_promotion p
                           ON p.prom_date <= Last_day(m.snapshot_month)
                              AND m.emp_id = p.emp_id
       GROUP  BY m.snapshot_month,
                 m.emp_id 

## FEAT_TRAINING

       SELECT m.snapshot_month,
              m.emp_id,
              /* Total training hours in last 6 months */
              Round(SUM(t.hours), 2) AS train_hh_6m,
              /* Average training assessment score */
              Round(Avg(t.score), 2) AS avg_train_score_6m,
              /* Total certifications completed */
              Count(CASE
                      WHEN t.completed = 'Y' THEN 1
                    END)             AS cert_count
       FROM   perf_emp_snap_month m
              left outer join perf_fact_training t
                           ON t.emp_id = m.emp_id
                              AND t.key_date BETWEEN
                                  Add_months( Last_day(m.snapshot_month),
                                  -6 )
                                  + 1 AND Last_day(m.snapshot_month)
       GROUP  BY m.snapshot_month,
                 m.emp_id 

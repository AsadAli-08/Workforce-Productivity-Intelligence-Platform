# Data Validation

## Check manpower count for each snapshot
    SELECT snapshot_month,
           Count(*)
    FROM   perf_fact_monthly_snapshot
    GROUP  BY snapshot_month
    ORDER  BY snapshot_month; 

## Check duplicate emp_id for each snapshot 
    SELECT snapshot_month,
           Count(*)                          AS total_rows,
           Count(DISTINCT emp_id)            AS unique_employees,
           Count(*) - Count(DISTINCT emp_id) AS duplicate_count
    FROM   perf_fact_monthly_snapshot
    GROUP  BY snapshot_month
    HAVING Count(*) > Count(DISTINCT emp_id)
    ORDER  BY snapshot_month; 

## Primary Key NULL Check
    SELECT Count(*) AS null_primary_keys
    FROM   perf_fact_monthly_snapshot
    WHERE  snapshot_month IS NULL
            OR emp_id IS NULL; 

## Date Range Validation
    SELECT Min(snapshot_month) AS min_date,
           Max(snapshot_month) AS max_date
    FROM   perf_fact_monthly_snapshot; 

## Missing Snapshot Months
    SELECT emp_id,
           snapshot_month                                               AS
           curr_month,
           Lag(snapshot_month, 1)
             over (
               PARTITION BY emp_id
               ORDER BY snapshot_month)                                 AS
           prev_month,
           Months_between(snapshot_month, Lag(snapshot_month, 1)
                                            over (
                                              PARTITION BY emp_id
                                              ORDER BY snapshot_month)) AS
           month_diff
    FROM   perf_fact_monthly_snapshot; 

## CONSISTENCY CHECKS

### Tasks Completed Cannot Exceed Assigned
    SELECT Count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  tasks_completed > tasks_assigned;

### Total compensation validation
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  basic + bonus + incentive > total;

### Leave Components Validation
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  sick_leaves + casual_leaves > absence_days;

### Productive Hours vs Work Hours
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  produc_hours > work_hours;

## OUTLIER CHECKS
### Extreme Overtime Detection
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  overtime_hh > 100;

### Extreme Productivity Detection
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  prod_rate < 40
    OR     prod_rate > 100;

## TEMPORAL CHECKS
### No Records Before DOJ
    SELECT count(*)
    FROM   perf_fact_monthly_snapshot
    WHERE  last_day(snapshot_month) < doj
    OR     last_day(snapshot_month) > dor;


## NULL count analysis
SELECT
    COUNT(*) AS TOTAL_ROWS,
    count(*) - count(LOCATION),
    count(*) - count(GRADE),
    count(*) - count(TENURE_YY),
    count(*) - count(AGE_YY),
    count(*) - count(TASKS_ASSIGNED),
    count(*) - count(TASKS_COMPLETED),
    count(*) - count(PROD_RATE),
    count(*) - count(TIMELINESS),
    count(*) - count(ERROR_RATE),
    count(*) - count(UTILISE_RATE),
    count(*) - count(PRODUC_HOURS),
    count(*) - count(RATING),
    count(*) - count(KPI),
    count(*) - count(GOAL_PERC),
    count(*) - count(MGR_RATING),
    count(*) - count(POTENTIAL),
    count(*) - count(PERF_CHANGE),
    count(*) - count(WORKING_DAYS),
    count(*) - count(ABSENCE_DAYS),
    count(*) - count(SICK_LEAVES),
    count(*) - count(CASUAL_LEAVES),
    count(*) - count(LATE_LOGINS),
    count(*) - count(EARLY_EXITS),
    count(*) - count(OVERTIME_HH),
    count(*) - count(TRAIN_HH_6M),
    count(*) - count(AVG_TRAIN_SCORE_6M),
    count(*) - count(CERT_COUNT),
    count(*) - count(BASIC),
    count(*) - count(BONUS),
    count(*) - count(INCENTIVE),
    count(*) - count(TOTAL),
    count(*) - count(COMPA_RATIO),
    count(*) - count(SAL_GROWTH),
    count(*) - count(YY_SINCE_PROM),
    count(*) - count(ENGAGE_SCORE),
    count(*) - count(BURN_SCORE),
    count(*) - count(WELL_SCORE),
    count(*) - count(MANAGER_SCORE),
    count(*) - count(CULTURE_SCORE),
    count(*) - count(WORKLOAD_SCORE),
    count(*) - count(PROD_3M_AVG),
    count(*) - count(PROD_3M_SLOPE),
    count(*) - count(PROD_VOLATILITY),
    count(*) - count(OVERTIME_3M_AVG),
    count(*) - count(ABSENCE_3M_AVG),
    count(*) - count(CONS_OVERTIME_MONTHS),
    count(*) - count(LEAVE_SPIKE_FLAG),
    count(*) - count(COMP_STAG_FLAG),
    count(*) - count(PAY_GROWTH_TREND),
    count(*) - count(stagflation),
    count(*) - count(Career_velocity),
    count(*) - count(Prod_risk_flag),
    count(*) - count(Burnout_risk_flag),
    count(*) - count(High_prod_flag)
FROM perf_FACT_MONTHLY_snapshot;

## Min/max analysis
SELECT

    /* Attendance */
    MIN(sick_leaves) AS min_sick_leaves,
    MAX(sick_leaves) AS max_sick_leaves,

    MIN(casual_leaves) AS min_casual_leaves,
    MAX(casual_leaves) AS max_casual_leaves,

    MIN(late_logins) AS min_late_logins,
    MAX(late_logins) AS max_late_logins,

    MIN(early_exits) AS min_early_exits,
    MAX(early_exits) AS max_early_exits,

    MIN(overtime_hh) AS min_overtime_hh,
    MAX(overtime_hh) AS max_overtime_hh,

    /* Training */
    MIN(train_hh_6m) AS min_train_hh_6m,
    MAX(train_hh_6m) AS max_train_hh_6m,

    MIN(avg_train_score_6m) AS min_avg_train_score_6m,
    MAX(avg_train_score_6m) AS max_avg_train_score_6m,

    MIN(cert_count) AS min_cert_count,
    MAX(cert_count) AS max_cert_count,

    /* Productivity Features */
    MIN(prod_3m_avg) AS min_prod_3m_avg,
    MAX(prod_3m_avg) AS max_prod_3m_avg,

    MIN(prod_3m_slope) AS min_prod_3m_slope,
    MAX(prod_3m_slope) AS max_prod_3m_slope,

    MIN(prod_volatility) AS min_prod_volatility,
    MAX(prod_volatility) AS max_prod_volatility,

    MIN(prod_risk_flag) AS min_prod_risk_flag,
    MAX(prod_risk_flag) AS max_prod_risk_flag,

    /* Attendance Features */
    MIN(overtime_3m_avg) AS min_overtime_3m_avg,
    MAX(overtime_3m_avg) AS max_overtime_3m_avg,

    MIN(absence_3m_avg) AS min_absence_3m_avg,
    MAX(absence_3m_avg) AS max_absence_3m_avg,

    MIN(cons_overtime_months) AS min_cons_overtime_months,
    MAX(cons_overtime_months) AS max_cons_overtime_months,

    MIN(leave_spike_flag) AS min_leave_spike_flag,
    MAX(leave_spike_flag) AS max_leave_spike_flag,

    MIN(burnout_risk_flag) AS min_burnout_risk_flag,
    MAX(burnout_risk_flag) AS max_burnout_risk_flag,

    /* Compensation */
    MIN(pay_growth_trend) AS min_pay_growth_trend,
    MAX(pay_growth_trend) AS max_pay_growth_trend,

    MIN(comp_stag_flag) AS min_comp_stag_flag,
    MAX(comp_stag_flag) AS max_comp_stag_flag,

    /* Demographics */
    MIN(snapshot_month) AS min_snapshot_month,
    MAX(snapshot_month) AS max_snapshot_month,

    MIN(tenure_yy) AS min_tenure_yy,
    MAX(tenure_yy) AS max_tenure_yy,

    MIN(age_yy) AS min_age_yy,
    MAX(age_yy) AS max_age_yy,

    /* Career */
    MIN(stagflation) AS min_stagflation,
    MAX(stagflation) AS max_stagflation,

    MIN(career_velocity) AS min_career_velocity,
    MAX(career_velocity) AS max_career_velocity,

    /* Productivity Operational */
    MIN(tasks_assigned) AS min_tasks_assigned,
    MAX(tasks_assigned) AS max_tasks_assigned,

    MIN(tasks_completed) AS min_tasks_completed,
    MAX(tasks_completed) AS max_tasks_completed,

    MIN(prod_rate) AS min_prod_rate,
    MAX(prod_rate) AS max_prod_rate,

    MIN(timeliness) AS min_timeliness,
    MAX(timeliness) AS max_timeliness,

    MIN(error_rate) AS min_error_rate,
    MAX(error_rate) AS max_error_rate,

    MIN(work_hours) AS min_work_hours,
    MAX(work_hours) AS max_work_hours,

    MIN(utilise_rate) AS min_utilise_rate,
    MAX(utilise_rate) AS max_utilise_rate,

    MIN(produc_hours) AS min_produc_hours,
    MAX(produc_hours) AS max_produc_hours,

    MIN(high_prod_flag) AS min_high_prod_flag,
    MAX(high_prod_flag) AS max_high_prod_flag,

    /* Performance */
    MIN(rating) AS min_rating,
    MAX(rating) AS max_rating,

    MIN(kpi) AS min_kpi,
    MAX(kpi) AS max_kpi,

    MIN(goal_perc) AS min_goal_perc,
    MAX(goal_perc) AS max_goal_perc,

    MIN(mgr_rating) AS min_mgr_rating,
    MAX(mgr_rating) AS max_mgr_rating,

    MIN(potential) AS min_potential,
    MAX(potential) AS max_potential,

    MIN(perf_change) AS min_perf_change,
    MAX(perf_change) AS max_perf_change,

    /* Salary */
    MIN(basic) AS min_basic,
    MAX(basic) AS max_basic,

    MIN(bonus) AS min_bonus,
    MAX(bonus) AS max_bonus,

    MIN(incentive) AS min_incentive,
    MAX(incentive) AS max_incentive,

    MIN(total) AS min_total,
    MAX(total) AS max_total,

    MIN(compa_ratio) AS min_compa_ratio,
    MAX(compa_ratio) AS max_compa_ratio,

    MIN(yy_since_prom) AS min_yy_since_prom,
    MAX(yy_since_prom) AS max_yy_since_prom,

    /* Engagement */
    MIN(engage_score) AS min_engage_score,
    MAX(engage_score) AS max_engage_score,

    MIN(burn_score) AS min_burn_score,
    MAX(burn_score) AS max_burn_score,

    MIN(well_score) AS min_well_score,
    MAX(well_score) AS max_well_score,

    MIN(manager_score) AS min_manager_score,
    MAX(manager_score) AS max_manager_score,

    MIN(culture_score) AS min_culture_score,
    MAX(culture_score) AS max_culture_score,

    /* Additional */
    MIN(sal_growth) AS min_sal_growth,
    MAX(sal_growth) AS max_sal_growth,

    MIN(working_days) AS min_working_days,
    MAX(working_days) AS max_working_days,

    MIN(absence_days) AS min_absence_days,
    MAX(absence_days) AS max_absence_days,

    MIN(workload_score) AS min_workload_score,
    MAX(workload_score) AS max_workload_score

FROM perf_fact_monthly_snapshot;



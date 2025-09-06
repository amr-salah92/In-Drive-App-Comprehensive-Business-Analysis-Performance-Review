# In-Drive App: Comprehensive Business Analysis & Performance Review

## Table of Contents
- [Project Overview](#project-overview)
- [Project Background](#project-background)
- [Project Goals](#project-goals)
- [Key Analysis Areas](#key-analysis-areas)
- [Data Collection and Sources](#data-collection-and-sources)
- [Formal Data Governance](#formal-data-governance)
- [Regulatory Reporting](#regulatory-reporting)
- [Methodology](#methodology)
- [Data Structure & Initial Checks](#data-structure--initial-checks)
- [Documenting Issues](#documenting-issues)
- [Executive Summary](#executive-summary)
- [Insights Deep Dive](#insights-deep-dive)
- [Recommendations](#recommendations)
- [Future Work](#future-work)
- [Technical Details](#technical-details)
- [Assumptions and Caveats](#assumptions-and-caveats)

## Project Overview
**Company**: In drive App  
**Analysis**: Comprehensive Performance Review (September 2023 - September 2025)  
**Focus Areas**: Financial Performance, User Growth, Operational Efficiency, and Driver Ecosystem

## Project Background
The company operates a ride-hailing platform in Saudi Arabia, serving major cities including Riyadh, Jeddah, Dammam, Makkah, and Dhahran. Since 2020, the platform has completed over 50,000 trips with 16,517 completed rides generating approximately 540K SAR in commission revenue. The business model connects riders with drivers through a mobile application, taking a commission on each completed ride (average 32.7 SAR per trip). Key metrics include monthly active users (stable at 390-450 new users monthly), driver count, completion rate (33% of initiated trips), and commission-to-payout ratio (66.67% company share).

## Project Goals
This analysis aims to identify operational inefficiencies, financial risks, and growth opportunities by examining:
1. Revenue stability and commission trends across cities and ride types
2. User acquisition and retention patterns to address churn issues
3. Driver performance and satisfaction indicators affecting service quality
4. Payment system reliability and technical issues impacting user experience
5. Operational metrics including cancellation rates (67% of initiated trips) and support case resolution times (11-12 hours average)

## Key Analysis Areas
- **Financial Performance**: Commission trends, payout ratios, and wallet transactions
- **User Growth & Loyalty**: Acquisition patterns, retention issues, and loyalty program effectiveness
- **Operational Efficiency**: Trip completion rates, cancellation factors, and geographic coverage
- **Driver Ecosystem**: Performance metrics, compliance issues, and support case management

## Data Collection and Sources
**Primary Sources**: 
- Internal PostgreSQL database containing trip records, user accounts, driver information, and financial transactions
- Payment processing systems (Apple Pay, credit cards, wallet, cash)
- Customer support ticketing system (Zendesk API integration)
- Driver onboarding and compliance documentation

**External Sources**:
- Geographic data for city and location mapping
- Payment gateway APIs (Apple Pay, credit card processors)
- Regulatory databases for driver compliance verification

## Formal Data Governance
The company implements a basic data governance framework with standardized:
- User ID formats across all systems
- Timestamp standardization (UTC+3 timezone)
- Payment status categorization
- Trip status classifications (completed, cancelled, no-show)

**Areas for Improvement**:
- Phone number format standardization (currently multiple formats)
- Driver documentation compliance (30% missing Iqama IDs)
- Cancellation reason categorization (currently illogical classifications)
- Wallet transaction anomaly detection systems

## Regulatory Reporting
The platform maintains compliance with Saudi Arabian transportation regulations through:
- Driver documentation verification (Iqama ID, vehicle registration)
- Ride tracking and receipt generation for all completed trips
- Financial reporting for commission transactions
- Data privacy protections for user information

**Addressing Data Inconsistencies**:
- Regular audits of driver documentation compliance
- Validation checks for financial transaction anomalies
- Geographic data verification for service area compliance

## Methodology
The analysis employed descriptive analytics, trend analysis, and correlation studies using:
- SQL for data extraction and aggregation
- Python (Pandas, NumPy) for data cleaning and transformation
- Tableau for visualization and dashboard creation
- Statistical analysis for identifying significant patterns and anomalies

No machine learning models were applied in this initial analysis, though the foundation has been set for predictive analytics in future work.

## Data Structure & Initial Checks
The company's database consists of 8 primary tables with approximately 250,000 total records:

**Table 1: trips**
- Columns: trip_id, user_id, driver_id, start_time, end_time, pickup_location, dropoff_location, fare_amount, commission_amount, status, payment_method, rating
- Represents: Core trip information and financial transactions
- Key Points: 50,000 total trips, 33% completion rate, 67% cancellation/no-show rate
- Usage Considerations: Status field requires recategorization for accurate analysis

**Table 2: users**
- Columns: user_id, signup_date, phone_number, country_of_origin, loyalty_points, status
- Represents: Customer account information and loyalty metrics
- Key Points: 61 months of acquisition data, 99.9% loyalty point redemption rate
- Usage Considerations: Phone number standardization needed, inactive status tracking

**Table 3: drivers**
- Columns: driver_id, join_date, car_make, car_model, iqama_id, phone_number, rating, status
- Represents: Driver information and vehicle details
- Key Points: 30% missing Iqama IDs, 0.1% missing car models, suspension status tracking
- Usage Considerations: Compliance issues need addressing before analysis

**Table 4: support_cases**
- Columns: case_id, user_id, driver_id, trip_id, issue_category, open_time, resolve_time, status, escalation_status
- Represents: Customer support interactions and issue resolution
- Key Points: 33% escalation rate, 11-12 hour resolution time, categorization issues
- Usage Considerations: Issue categories need standardization for accurate reporting

**Table 5: wallet_transactions**
- Columns: transaction_id, driver_id, amount, transaction_type, timestamp, status
- Represents: Financial movements in driver wallets
- Key Points: 94,593 SAR net outflow, high concentration among top drivers
- Usage Considerations: September 2025 anomaly requires investigation

**Table 6: payments**
- Columns: payment_id, trip_id, method, amount, status, issue_flag
- Represents: Payment processing information
- Key Points: Apple Pay most used method (20.39%), wallet has highest issue rate (28.48%)
- Usage Considerations: Payment issue patterns need technical investigation

**Table 7: cities**
- Columns: city_id, city_name, location_count, operational_status
- Represents: Geographic service areas
- Key Points: Dammam has most locations (85), Makkah has fewest (53)
- Usage Considerations: Location data needs verification for accuracy

**Table 8: promotions**
- Columns: promo_id, user_id, trip_id, discount_amount, redemption_date
- Represents: Marketing promotions and discounts
- Key Points: 4.9% redemption rate, 800% ROI on promotions
- Usage Considerations: Limited data requires careful interpretation of effectiveness

[Entity Relationship Diagram would be inserted here]

## Documenting Issues

| Table | Column | Issues | Magnitude | Solvable Status | Resolutions |
|-------|--------|--------|-----------|----------------|-------------|
| drivers | iqama_id | 30% of records missing | High | Yes | Implement mandatory field validation in driver onboarding |
| drivers | car_model | 0.1% of records missing | Medium | Yes | Add data validation checks during vehicle registration |
| users | phone_number | Multiple format inconsistencies | Medium | Yes | Implement standardization script and validation rules |
| trips | status | Cancellation reasons illogical | High | Yes | Redesign cancellation reason categories and tracking |
| wallet_transactions | amount | September 2025 anomaly (-903K SAR) | Critical | Yes | Immediate investigation and data correction |
| support_cases | issue_category | Inconsistent categorization | High | Yes | Standardize categories and retrain support team |
| trips | wait_time | Associated with no-shows when >2.4 mins | High | Yes | Optimize dispatch algorithms to reduce wait times |
| all_tables | September 2024-2025 | Data anomalies in specific months | High | Partial | Investigate system issues during these periods |

## Executive Summary
**For**: Chief Operations Officer and Finance Director

The platform demonstrates stable user acquisition but critical retention issues, with 67% of initiated trips failing to complete. Financial analysis reveals a concerning liquidity risk with 94,593 SAR net outflow from driver wallets, concentrated among a small group of drivers. Operational efficiency is hampered by long support resolution times (11-12 hours) and 33% case escalation rates. While geographic profitability is consistent (~40% margins across cities), payment system issues particularly affect wallet transactions (28.48% issue rate). Immediate actions are needed to address data integrity issues, particularly the September 2025 financial anomaly and driver documentation gaps.

![Screenshot_6-9-2025_203035_chat deepseek com](https://github.com/user-attachments/assets/b9847cea-c722-46d2-93fb-9e3a90cfcdde)


## Insights Deep Dive

### Category 1: Financial Performance

**Main insight 1**: The company maintains a 66.67% commission share but faces liquidity risk with 94,593 SAR net outflow from driver wallets, indicating the current model may be unsustainable. Driver payouts (809,577 SAR) exceed deposits (809,577 SAR), requiring operational reserves to cover withdrawals.

**Main insight 2**: September 2025 showed an anomalous transaction of -903,575.87 SAR that distorts financial reporting. This outlier requires immediate investigation as either a data error or genuine financial adjustment before accurate trend analysis can proceed.

**Main insight 3**: Wallet usage is highly concentrated among 5 drivers (IDs 4493, 8905, 1841, 6184, 5457), creating dependency risk. If these drivers churn, transaction volume could significantly decrease, worsening liquidity outflow.

**Main insight 4**: Commission trends show seasonal patterns with October peaks (+13.73% in 2023, +10.94% in 2024) followed by November declines. This pattern suggests opportunity for targeted campaigns during growth periods and defensive strategies during slowdowns.

![Screenshot_6-9-2025_193839_chat deepseek com](https://github.com/user-attachments/assets/588fec3b-bee3-478b-8660-b5d0f8d9c230)


### Category 2: User Growth & Loyalty

**Main insight 1**: User acquisition has remained stable (390-450 new users monthly) since 2020, but September 2024 showed a significant dip that requires investigation into potential platform issues or competitive actions.

**Main insight 2**: A loyalty paradox exists where redemption rate is 99.9% but monthly churn remains high (130-200 users monthly). This indicates that while users redeem points, the program doesn't effectively drive long-term retention.

**Main insight 3**: Inactive users are steadily increasing over time despite stable acquisition, confirming a "leaky bucket" scenario where retention efforts are not keeping pace with acquisition.

**Main insight 4**: Demographic trends show male users have higher ratings but lower loyalty points, Pakistani users have the highest ratings, and cash-paying users have high ratings but low loyalty points—indicating segmentation opportunities.

![Screenshot_6-9-2025_193818_chat deepseek com](https://github.com/user-attachments/assets/eb1bd1e9-21e6-4f2b-b353-52070d3bd862)


### Category 3: Operational Efficiency

**Main insight 1**: Only 33% of initiated trips complete, with 67% resulting in cancellations or no-shows. Wait times exceeding 2.4 minutes significantly increase cancellation likelihood, indicating dispatch optimization opportunities.

**Main insight 2**: Support case resolution averages 11-12 hours with 33% escalation rate, indicating process inefficiencies. Critical issues like safety concerns experience these same delays, creating potential risk exposures.

**Main insight 3**: Geographic analysis shows consistent ~40% margins across all cities, with Dammam (16.7% of trips) and Jeddah (13.3%) being highest volume cities. Makkah has the lowest share (10.6%) despite religious tourism potential.

**Main insight 4**: Promotions show exceptionally high ROI (~800%) but low adoption (4.9% of trips), suggesting significant opportunity for scaled promotional campaigns targeted at underperforming cities or user segments.



### Category 4: Driver Ecosystem

**Main insight 1**: Compliance issues exist with 30% of drivers missing Iqama IDs and 0.1% missing car model information, creating regulatory risk and service consistency issues.

**Main insight 2**: Premium car drivers (Hyundai, Nissan, Toyota representing 47% of premium category) show below-average ratings despite higher fare expectations, indicating potential service quality issues in premium segment.

**Main insight 3**: 8.2% of drivers are suspended due to safety concerns, and these drivers account for 33% of all support cases. A subset (7.12%) are both suspended and repeat problem-makers, indicating need for better screening or training.

**Main insight 4**: Top-performing drivers (IDs 4971, 8591, 7317, 6101, 3418) have no complaints and high ratings, providing opportunity for best practice identification and replication across the driver community.


![Screenshot_6-9-2025_193747_chat deepseek com](https://github.com/user-attachments/assets/073b7714-33c5-42c6-a6d7-cf5d6e3d1f06)

---

## **Recommendations**

### 1. **Customer Support Efficiency**

* Reduce the **average resolution time** (currently 11–12 hours) to align with industry best practices.
* Prioritize **serious issue categories** (e.g., safety concerns, disputes) with a fast-track resolution process.
* Work on resolving **open cases** quickly to prevent backlog.
* Focus on **escalated cases** caused by long waiting times (⅓ of support data).

---

### 2. **AI & Automation**

* Implement **AI chatbots** to handle simple queries and reduce initial response time.
* Use **automated triage** to assign cases to the right team/agent based on issue type.
* Apply **predictive analytics** to anticipate recurring issues and proactively resolve them.

---

### 3. **User & Driver Management**

* Reach out to **top-complaint users** (User IDs: 14066, 11929, 12537) to address recurring issues and improve satisfaction.
* Investigate **top-complaint drivers** (Driver IDs: 7360, 4229, 6322, 7570, 7636) for potential retraining, monitoring, or disciplinary action.
* Track **user/driver issue history** to identify repeat offenders or chronic dissatisfaction.

---

### 4. **Future Data Integration**

* **Merge customer support data with trip data** to link complaints to specific rides and measure impact on operations.
* **Merge with driver data** to analyze patterns of complaints tied to drivers, especially for safety and disputes.
* **Merge with user data** to profile customers with frequent complaints and design retention strategies.


## Future Work

1. **Driver Segmentation Analysis**: Conduct profitability analysis by segmenting drivers based on revenue generated, commission paid, and wallet activity to identify optimal driver profiles.

2. **Promotion Optimization Testing**: Implement A/B testing for promotional campaigns targeted at specific user segments and underperforming cities to maximize ROI from the current 800% baseline.

3. **Seasonality Pattern Analysis**: Conduct deep analysis of October peak and November decline patterns to determine causal factors and develop targeted strategies for each period.

4. **Payment System Technical Audit**: Comprehensive review of wallet and Apple Pay systems to address high issue rates (28.48% and 19.21% respectively) affecting user experience.

## Technical Details

**Programs Used**:

- Python (Pandas, NumPy) for data transformation and cleaning, leveraging their robust data manipulation capabilities
- Power bi for visualization and dashboard creation thanks to its interactive capabilities and user-friendly interface


The Python queries used to inspect and clean the data for this analysis can be found [here]([https://example.com/sql-queries](https://github.com/amr-salah92/In-Drive-App-Comprehensive-Business-Analysis-Performance-Review/blob/main/In-drive.ipynb)).


An interactive Power bi dashboard used to report and explore trends can be found [here]([https://example.com/tableau-dashboard](https://github.com/amr-salah92/In-Drive-App-Comprehensive-Business-Analysis-Performance-Review/blob/main/driving%20app.pbix)).



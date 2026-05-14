# Bank-of-America-Fictitious-Data-
Full ETL process using medallion architecture with ML model 
Nb: for full information regarding the project portfolio, vist my github account using the link.
Link: https://github.com/namasasu/Bank-of-America-Fictitious-Data-.git

Bank of America Data Analytics Project (Fictitious)
Project Objectives
This project aims to establish a comprehensive data analytics infrastructure for Bank of America using the medallion architecture pattern. The primary objectives include:
•	Customer Insights & Segmentation - Analyze customer behavior patterns, transaction trends, and account activities to identify high-value segments and personalization opportunities
•	Transaction Monitoring & Analysis - Track and analyze transaction volumes, patterns, and anomalies across different channels (ATM, online, mobile, branch)
•	Risk Assessment & Fraud Detection - Identify suspicious activities, unusual transaction patterns, and potential fraud indicators through data-driven analysis
•	Operational Efficiency - Monitor branch performance, service quality metrics, and resource utilization to optimize operations
•	Regulatory Compliance - Ensure data governance, audit trails, and reporting capabilities meet regulatory requirements (e.g., AML, KYC, Dodd-Frank)
•	Business Intelligence & Reporting - Provide executive dashboards and reports for strategic decision-making
Problem Statement
Bank of America handles millions of daily transactions across diverse products (checking, savings, credit cards, loans, investments) and channels. The current challenges include
1.	Data Fragmentation - Customer and transaction data exists in multiple systems with inconsistent formats and quality
2.	Limited Real-Time Insights - Delays in data processing hinder timely decision-making for fraud prevention and customer service
3.	Scalability Concerns - Growing data volumes require a robust, scalable architecture to handle petabyte-scale datasets
4.	Compliance Complexity - Regulatory requirements demand comprehensive audit trails, data lineage, and secure access controls
5.	Analytical Barriers - Business users need simplified access to trusted, curated datasets without navigating complex raw data structures
Background
Data Sources
The project will integrate data from multiple source systems:
•	Core Banking Systems - Account information, customer profiles, product holdings
•	Transaction Processing Systems - ATM transactions, online banking, mobile app, wire transfers, ACH payments
•	Credit Card Systems - Card transactions, authorization data, merchant information
•	Customer Relationship Management (CRM) - Customer interactions, service requests, complaints
•	Risk & Compliance Systems - Credit scores, fraud alerts, AML flags, regulatory filings
•	External Data - Credit bureaus, market data, economic indicators
Medallion Architecture Implementation
Bronze Layer (Raw Data)
•	Ingests raw data from source systems with minimal transformation
•	Preserves original data formats and structures for audit and compliance
•	Includes full history and change data capture (CDC) for tracking
•	Serves as the single source of truth
Silver Layer (Cleaned & Transformed)
•	Cleanses and standardizes data (e.g., consistent date formats, currency conversions)
•	Deduplicates records and resolves data quality issues
•	Enriches data with calculated fields and derived attributes
•	Implements business rules and data validation
•	Provides queryable datasets for analysts
Gold Layer (Business-Level Aggregates)
•	Creates pre-aggregated, business-ready datasets optimized for reporting
•	Implements key performance indicators (KPIs) and metrics
•	Builds customer 360 views, product summaries, and analytical marts
•	Optimized for dashboards, BI tools, and executive reporting
•	Enforces access controls based on business roles
Expected Outcomes
•	Reduced time-to-insight from days to minutes through optimized data pipelines
•	Improved data quality and consistency across the organization
•	Enhanced fraud detection capabilities with near real-time monitoring
•	Simplified access to trusted data for business users and data scientists
•	Scalable foundation supporting future advanced analytics and machine learning initiatives
•	Full compliance with regulatory requirements and audit readiness
Stakeholders
•	Business Users - Branch managers, product teams, marketing, customer service
•	Data & Analytics Teams - Data engineers, data scientists, business analysts
•	Risk & Compliance - Fraud prevention, AML officers, compliance officers
•	Technology Teams - IT infrastructure, security, architecture
•	Executive Leadership - C-suite requiring strategic insights and KPI dashboards
Data Quality Rules and Standards
Overview
Data quality is critical for regulatory compliance, accurate analytics, and operational decision-making.
________________________________________
Bronze Layer - Data Quality Rules
Purpose
Capture and preserve raw data with minimal validation.
Key Rules
1. Completeness Rules
•	All source records must be captured
•	Metadata fields required: source_system, ingestion_timestamp, file_name, record_id
•	Track rejected records in separate error tables
2. Technical Validation
•	File format integrity checks
•	Schema conformance to expected structure
•	No truncated or corrupted records
3. Audit Trail
•	Preserve original data format and values
•	Capture CDC operations
•	Maintain full history with effective dates
________________________________________
Silver Layer - Data Quality Rules
Customer Data Rules
Customer Identification
•	customer_id - NOT NULL, UNIQUE
•	ssn - NULL allowed, when present: 9 digits, encrypted
•	date_of_birth - NOT NULL, age between 18-120 years
•	email - Valid email format when present
Customer Profile
•	first_name, last_name - NOT NULL
•	address_line1 - NOT NULL for primary address
•	city, state, zip_code - NOT NULL, valid US postal codes
•	customer_status - IN (ACTIVE, INACTIVE, SUSPENDED, CLOSED)
KYC and AML Compliance
•	kyc_status - IN (VERIFIED, PENDING, FAILED, EXPIRED)
•	risk_rating - IN (LOW, MEDIUM, HIGH, CRITICAL)
•	aml_check_date - Must be within last 365 days
Transaction Data Rules
Transaction Identification
•	transaction_id - NOT NULL, UNIQUE
•	account_id - NOT NULL, must exist in accounts table
•	transaction_date - NOT NULL, cannot be future date
Transaction Amounts
•	amount - NOT NULL, DECIMAL(18,2)
•	balance_after greater than or equal to 0
•	Currency code - NOT NULL, ISO 4217 format
Transaction Types
•	transaction_type - IN (DEPOSIT, WITHDRAWAL, TRANSFER, PAYMENT, FEE, INTEREST)
•	channel - IN (ATM, ONLINE, MOBILE, BRANCH, ACH, WIRE)
•	status - IN (COMPLETED, PENDING, FAILED, REVERSED)
Fraud Detection Flags
•	Flag transactions greater than ten thousand dollars
•	Flag multiple transactions totaling over ten thousand within 24 hours
•	Flag transactions in unusual locations
•	Flag rapid succession of transactions
Account Data Rules
Account Identification
•	account_id - NOT NULL, UNIQUE
•	customer_id - NOT NULL, must exist in customers table
•	account_number - NOT NULL, UNIQUE, encrypted
Account Details
•	account_type - IN (CHECKING, SAVINGS, CREDIT_CARD, LOAN, MORTGAGE)
•	account_status - IN (ACTIVE, DORMANT, FROZEN, CLOSED)
•	current_balance - NOT NULL, DECIMAL(18,2)
Referential Integrity
•	All customer_id references must exist in customers table
•	All account_id references must exist in accounts table
•	No transactions without valid account
•	No accounts without valid customer
________________________________________
Gold Layer - Data Quality Rules
Aggregation Accuracy
•	Sum of transactions must reconcile with account balances
•	Daily transaction counts must match source system reports
Business Logic Validation
•	Customer lifetime value calculations consistently applied
•	Churn rates calculated using standard 90-day window
•	Risk scores normalized to 0-1000 range
Data Freshness
•	Real-time dashboards updated every 5 minutes
•	Daily reports available by 6 AM EST
•	Monthly reports finalized within 3 business days
________________________________________
Cross-Layer Quality Checks
Data Consistency
•	Customer counts consistent across Bronze, Silver, Gold
•	Transaction volumes reconcile across layers
Date and Time Standardization
•	All timestamps in UTC
•	Date formats consistent: YYYY-MM-DD
Duplicate Prevention
•	Primary keys enforced at all layers
•	Deduplication logic documented
________________________________________
Implementation Approach
1.	Bronze Layer: Soft validation - log issues but ingest all data
2.	Silver Layer: Hard validation - reject records failing critical rules
3.	Gold Layer: Business validation - alert on anomalies
Monitoring:
•	Automated data quality dashboards
•	Daily quality score reports
•	Alert thresholds for quality degradation
Remediation:
•	Automated fixes for known patterns
•	Manual review queue for ambiguous cases
•	Quarterly data quality review meetings

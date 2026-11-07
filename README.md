# Credit Risk Monitoring and Default Alert System (creditRisk)

- **Student Name:** Turashimye Kundwa Dorian
- **Student ID:** 29540
- **Course:** INSY 8311 – Database Development with PL/SQL
- **Instructor:** Eric Maniraguha
- **GitHub Repository:** https://github.com/DorianKundwa/dorian_29540_creditRisk.git

---

## 1. Problem Statement
Many microfinance institutions, SACCOs, and local lenders still review loan applications and track borrower repayments using basic spreadsheets. When a loan officer checks a customer's annual income, past default history, and debt load manually, mistakes happen easily. Borrowers sometimes get approved for loans that take up more than half of their annual income, which makes repayment difficult and drives up Non-Performing Loans (NPLs). Keeping records in flat files also means there is no automatic database rule to stop risky loans before disbursement, and no audit log showing who modified a loan record.

## 2. Project Objectives
### Main Objective
To build an Oracle Pluggable Database (PDB) with PL/SQL that stores borrower credit records, blocks unsafe loan applications automatically, and tracks loan default risk.

### Specific Objectives
1. Clean and normalize the raw Kaggle Credit Risk CSV dataset into 5 tables in Third Normal Form (3NF) inside an Oracle Pluggable Database.
2. Write PL/SQL triggers that block new loan entries when the debt-to-income ratio exceeds 50% and record every rejected attempt in an audit table.
3. Develop PL/SQL procedures, functions, cursors, and a package to calculate borrower risk scores and review active loans.
4. Connect a Microsoft Power BI dashboard directly to the Oracle database to display default rates by loan intent, credit grade, and income level.

## 3. Data Source
- **Dataset:** Credit Risk Dataset (Kaggle – Lao Tse)
- **Link:** https://www.kaggle.com/datasets/laotse/credit-risk-dataset
- **Details:** 32,581 records across 12 columns (`person_age`, `person_income`, `person_home_ownership`, `person_emp_length`, `loan_intent`, `loan_grade`, `loan_amnt`, `loan_int_rate`, `loan_status`, `loan_percent_income`, `cb_person_default_on_file`, `cb_preson_cred_hist_length`). Shared under an open license for educational use with no personal names, national IDs, or private contact details.

## 4. Planned Tables (3NF)
1. `BORROWERS` – Holds borrower profile and credit bureau history (`borrower_id` PK, `person_age`, `person_income`, `home_ownership`, `emp_length_years`, `cred_hist_years`, `default_on_file`).
2. `LOAN_GRADES` – Lookup table for credit grades A through G (`grade_code` PK, `risk_level`, `standard_int_rate`, `max_allowed_dti`).
3. `LOANS` – Stores individual loan applications (`loan_id` PK, `borrower_id` FK, `grade_code` FK, `loan_intent`, `loan_amount`, `interest_rate`, `loan_percent_income`, `loan_status`).
4. `LOAN_RISK_REVIEWS` – Records periodic risk checks and status updates (`review_id` PK, `loan_id` FK, `calculated_risk_score`, `review_status`, `review_date`).
5. `CREDIT_AUDIT_LOG` – Tracks blocked loan insertions and user changes (`audit_id` PK, `table_name`, `action_type`, `db_user`, `action_date`, `details`).

## 5. Planned PL/SQL Features
- **Package (`pkg_credit_risk`):** Keeps the loan review procedures and borrower risk score functions organized in one place.
- **Procedure (`proc_review_loan_status`):** Updates a loan's status in `LOANS` and writes a review entry into `LOAN_RISK_REVIEWS`.
- **Function (`fn_get_borrower_risk_score`):** Takes a `borrower_id` and returns a risk score from 0 to 100 based on their debt-to-income ratio, loan grade, and prior default record.
- **Cursor (`cur_high_risk_loans`):** Loops through active loans where `loan_percent_income > 0.40` to flag them for follow-up.
- **Triggers (`trg_block_high_dti_loan` & `trg_log_loan_changes`):** A `BEFORE INSERT` trigger on `LOANS` stops a new loan if `loan_percent_income > 0.50` (over 50% of annual income) or if age/employment values are invalid, and logs the blocked attempt in `CREDIT_AUDIT_LOG`.

## 6. Innovation
- **Option:** Analytics dashboard (Power BI, Tableau)
- **Tool:** Microsoft Power BI connected directly to the Oracle database to filter default rates by loan intent (Education, Medical, Venture, Personal), compare credit grades, and view blocked loan attempts from `CREDIT_AUDIT_LOG`.

## 7. References (APA)
- Basel Committee on Banking Supervision. (2020). *Guidelines for credit risk management*. Bank for International Settlements. https://www.bis.org/
- Lao Tse. (2020). *Credit risk dataset* [Data set]. Kaggle. https://www.kaggle.com/datasets/laotse/credit-risk-dataset
- Mifos Initiative. (2024). *Mifos X open-source core banking system documentation*. https://mifos.org/
- National Bank of Rwanda. (2021). *Assessing the systemic importance of banks in Rwanda* (BNR Economic Review, Vol. 17). https://www.bnr.rw/
- Ndikubwimana, P., et al. (2023). Credit risk analysis and microfinance loan quality in Rwanda: A case study of COPEDU Ltd. *ResearchGate*. https://www.researchgate.net/publication/372428843
- Oracle Corporation. (2024). *Oracle Database PL/SQL language reference (21c)*. Oracle Help Center. https://docs.oracle.com/en/database/oracle/oracle-database/21/lnpls/
- Rwanda Bankers' Association. (2024). *State of the banking industry report 2024*. https://rba.rw/

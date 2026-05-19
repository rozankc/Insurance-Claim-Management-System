# Insurance Claim Management System
## Final Project Report

> **Course:** INFO 5707.401 — Data Modelling for Information Professionals
> **Institution:** University of North Texas, Department of Information Science

---

## Table of Contents

- [Project Description](#-project-description)
- [Scope](#-scope)
- [Project Requirements](#-project-requirements)
- [Business Rules](#-business-rules)
- [Entity Relationship Diagram](#-entity-relationship-diagram)
- [Table Relationships](#-table-relationships)
- [Data Dictionary](#-data-dictionary)
- [Entity Generation & Schema](#-entity-generation--schema)
- [Data Analysis](#-data-analysis)
- [Acknowledgments](#-acknowledgments)

---

## Project Description

The **Insurance Claim Management System (CMS)** is a sophisticated and comprehensive solution aimed at optimizing the way insurance companies manage their claims. Designed with scalability and efficiency in mind, it addresses critical problems that insurance providers face in their day-to-day work.

By utilizing modern database technologies, the system integrates claim data with transactional data and policy details into a **unified database** — eliminating data silos and ensuring consistency and accuracy throughout the claim lifecycle.

**Objective:**
From the initial First Notice of Loss (FNOL) to final claim settlement, this CMS provides a robust framework to analyze and manage claims. It enables customer service representatives to document claim information accurately, reducing errors and processing time. The system also enforces role-based access control and provides real-time data visibility to improve operational reliability.

---

## Scope

The system manages the full insurance claim lifecycle:

- **Claim Initiation** — Unique claim numbers are assigned at FNOL
- **Policy Linkage** — Each claim connects to a policy table holding customer coverage details
- **Exposure Tracking** — Losses are categorized into exposures that define coverage type applied
- **Financial Processing** — Exposures link to the Financials table for accurate reimbursements
- **Audit Trail** — All create, modify, and delete actions are logged in the History table
- **Role-Based Access** — Only authorized personnel can access confidential records

---

## Project Requirements

### Operating Environment

| Component | Technology |
|-----------|-----------|
| Operating System | Windows |
| Database | MySQL 8.0 |
| IDE | MySQL Workbench |
| Diagramming | Draw.io |
| Data Preparation | Microsoft Excel |
| Documentation | Microsoft Word |

### Database Tables

| # | Table |
|---|-------|
| 1 | `Claim` |
| 2 | `Policy` |
| 3 | `PolicyCoverage` |
| 4 | `CoveredPeople` |
| 5 | `VehicleDetails` |
| 6 | `History` |
| 7 | `Employer` |
| 8 | `Exposure` |
| 9 | `FNOL` |
| 10 | `Financial` |
| 11 | `LossType` |
| 12 | `LossState` |

### User Requirements

| Requirement | Description |
|-------------|-------------|
| Claim Registration | Representatives can open new claims automatically flagged as FNOL |
| FNOL Information | Date of loss and policy number must be submitted when initiating a claim |
| Vehicle & Casualty Info | Vehicle damage claims require at least one injury or damage detail |
| Exposure Creation | Exposures calculated based on underwritten policy coverage amounts |
| Exposure Editing | Agents can update claimed amount, coverage type, and loss details |
| Payment Logging | All transactions must be tracked and linked to claims |
| Payment Constraints | Compensation cannot exceed the covered amount |
| Coverage Verification | Agents can verify insurance directly from policy when creating exposures |
| History Immutability | Past history events cannot be modified |
| Active Policy Requirement | Claims can only be filed under active policies |
| Claim Summary | Agents can access claim and exposure summaries with status (open/closed/pending) |

---

## Business Rules

| Rule | Description |
|------|-------------|
| Unique Claim IDs | Each claim has a distinct number for efficient tracking |
| Mandatory Claim Info | Policy number, claim amount, and incident date are required |
| Coverage Verification | Claims must be verified against policy coverage before processing |
| Status Tracking | Claims follow: `Pending` → `In Review` → `Approved` / `Rejected` |
| Role-Based Access | Access to view or edit claims is based on employee role |
| Audit Logging | All edits are recorded with editor identity and timestamp |
| Duplicate Prevention | Mechanisms prevent duplicate claims for the same incident |
| Document Linking | Incident reports and medical documents are stored and linked to claims |
| Regulatory Compliance | System follows data protection policies for customer privacy |
| Payment Authorization | Payments require designated staff approval before disbursement |

---

## Entity Relationship Diagram

<!-- REPLACE with your ERD image -->
<p align="center">
  <img src="Images/ERD.png" alt="Entity Relationship Diagram (ERD)" width="700"/>
  <br/>
  <em>Entity Relationship Diagram (ERD) for the Insurance Claim Management System</em>
</p>

The ERD includes **12 entities** capturing all aspects of the claim process — from claims and policies, to exposures, financial transactions, audit history, and FNOL.

---

## Table Relationships

| Relationship | Type |
|--------------|------|
| `Employer` ↔ `History` | One to Many (1:M) |
| `Claim` ↔ `History` | One to Many (1:M) |
| `Claim` ↔ `Policy` | One to One (1:1) |
| `Claim` ↔ `Exposure` | One to Many (1:M) |
| `Policy` ↔ `PolicyCoverage` | One to Many (1:M) |
| `Policy` ↔ `VehicleDetails` | One to Many (1:M) |
| `Policy` ↔ `CoveredPeople` | One to Many (1:M) |
| `Exposure` ↔ `PolicyCoverage` | One to Many (1:M) |
| `Exposure` ↔ `Financial` | One to Many (1:M) |
| `Exposure` ↔ `FNOL` | One to One (1:1) |
| `FNOL` ↔ `LossState` | One to One (1:1) |
| `FNOL` ↔ `LossType` | One to One (1:1) |

---

## Data Dictionary

The data dictionary defines all data elements — names, types, formats — to ensure consistency and standardization across the system.

### Claim

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `claim_Number` | VARCHAR(30) | PK | Unique claim identifier |
| `policy_id` | VARCHAR(30) | FK | References Policy |
| `history_id` | VARCHAR(30) | FK | References History |
| `exposure_id` | VARCHAR(30) | FK | References Exposure |
| `FNOL_ID` | VARCHAR(30) | FK | References FNOL |
| `claimant_Fname` | VARCHAR(255) | — | Claimant first name |
| `claimant_Lname` | VARCHAR(255) | — | Claimant last name |
| `claim_State` | VARCHAR(10) | — | `ACTIVE` or `INACTIVE` |

### Policy

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `policy_id` | VARCHAR(30) | PK | Unique policy identifier |
| `policyNumber` | VARCHAR(30) | — | Policy reference number |
| `active` | VARCHAR(10) | — | `1` = Active, `0` = Inactive |
| `policyHolderName` | VARCHAR(255) | — | Full name of policyholder |
| `policyCoverage_id` | VARCHAR(30) | FK | References PolicyCoverage |
| `coveredPeople_id` | VARCHAR(30) | FK | References CoveredPeople |
| `vehicle_id` | VARCHAR(30) | FK | References VehicleDetails |

### PolicyCoverage

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `policyCoverage_id` | VARCHAR(30) | PK | Unique coverage identifier |
| `coverageName` | VARCHAR(30) | — | Coverage type (e.g., Collision) |
| `coveredAmount` | FLOAT | — | Maximum covered monetary amount |
| `deductible` | FLOAT | — | Applicable deductible |

### CoveredPeople

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `coveredPeople_id` | VARCHAR(30) | PK | Unique identifier |
| `firstName` | VARCHAR(255) | — | First name |
| `lastName` | VARCHAR(255) | — | Last name |
| `relation` | VARCHAR(255) | — | Relation to policyholder |

### VehicleDetails

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `vehicle_id` | VARCHAR(30) | PK | Unique vehicle identifier |
| `vehicleName` | VARCHAR(30) | — | Vehicle make/name |
| `model` | VARCHAR(20) | — | Vehicle model |
| `yearManufactured` | DATE | — | Year of manufacture |
| `color` | VARCHAR(20) | — | Vehicle color |

### History

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `history_id` | VARCHAR(30) | PK | Unique history record |
| `employee_id` | VARCHAR(30) | FK | References Employer |
| `created` | DATE | — | Date record was created |
| `edited` | DATE | — | Date record was last edited |
| `deleted` | DATE | — | Date record was deleted |

### Employer

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `employee_id` | VARCHAR(30) | PK | Unique employee identifier |
| `firstName` | VARCHAR(255) | — | First name |
| `lastName` | VARCHAR(255) | — | Last name |
| `email` | VARCHAR(255) | UNIQUE | Email address |
| `phoneNumber` | INT | UNIQUE | Phone number |

### Exposure

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `exposure_id` | VARCHAR(30) | PK | Unique exposure identifier |
| `exposureName` | VARCHAR(255) | — | Description of exposure |
| `policyCoverage_id` | VARCHAR(30) | FK | References PolicyCoverage |
| `FNOL_id` | VARCHAR(30) | FK | References FNOL |
| `financials_id` | VARCHAR(30) | FK | References Financial |
| `exposureState` | VARCHAR(10) | — | Current exposure state |

### FNOL

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `FNOL_id` | VARCHAR(30) | PK | Unique FNOL identifier |
| `lossType_id` | VARCHAR(30) | FK | References LossType |
| `lossCause` | VARCHAR(255) | — | Cause of loss |
| `severity` | VARCHAR(10) | — | `LOW` / `MEDIUM` / `HIGH` |

### Financial

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `financial_id` | VARCHAR(30) | PK | Unique financial record |
| `paymentType` | VARCHAR(255) | — | Cash, Card, Check, etc. |
| `paymentAmmount` | FLOAT | — | Payment amount |
| `paymentStatus` | VARCHAR(10) | — | `Paid` / `Unpaid` / `Pending` |
| `deductibleApplied` | FLOAT | — | Deductible applied to payment |

### LossType

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `lossType_id` | VARCHAR(30) | PK | Unique loss type identifier |
| `lossTypeName` | VARCHAR(255) | — | Name of the loss type |
| `lossCategory` | VARCHAR(255) | — | Loss category |

### LossState

| Column | Type | Key | Description |
|--------|------|-----|-------------|
| `StateCode` | VARCHAR(5) | PK | State abbreviation (e.g., `CA`) |
| `StateName` | VARCHAR(40) | — | Full state name |

---

## Entity Generation & Schema

### Loading Data from CSV

All tables are populated using the `LOAD DATA INFILE` command. Example for the Claim table:

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/Claim.csv'
INTO TABLE Claim
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

> Repeat for each CSV in the `Data/` folder. Always load **parent tables before child tables** to respect foreign key constraints.

**Recommended load order:**

```
LossType → LossState → Employer → Financial → PolicyCoverage
→ CoveredPeople → VehicleDetails → Policy → FNOL → Exposure → History → Claim
```

---

## Data Analysis

Ten SQL queries were developed to extract business insights. Results support decision-making across operations, HR, and finance.

---

### Analysis 1 — Total Claims per Policy

**Purpose:** Identify claim patterns to reassess policy premiums.

```sql
SELECT p.policy_id, COUNT(c.policy_id) AS claim_count
FROM policy p
LEFT JOIN claim c ON p.policy_id = c.policy_id
GROUP BY p.policy_id
LIMIT 0, 1000;
```

<p align="center"><img src="Images/Picture1.png" alt="Analysis 1" width="700"/></p>

---

### Analysis 2 — Top Claim-Processing Employees

**Purpose:** Evaluate and reward top-performing employees.

```sql
SELECT e.employee_id, e.firstName, e.lastName,
       COUNT(c.history_id) AS claims_processed
FROM employer e
JOIN history h ON e.employee_id = h.employee_id
JOIN claim c   ON h.history_id  = c.history_id
GROUP BY e.employee_id
ORDER BY claims_processed DESC
LIMIT 5;
```
<p align="center"><img src="Images/Picture2.png" alt="Analysis 2" width="700"/></p>

---

### Analysis 3 — Claims with Missing Exposure Details

**Purpose:** Detect data entry gaps for quality control.

```sql
SELECT c.claim_Number, c.policy_id, c.exposure_id
FROM claim c
LEFT JOIN exposure e ON c.exposure_id = e.exposure_id
WHERE e.exposure_id IS NULL;
```

<p align="center"><img src="Images/Picture3.png" alt="Analysis 3" width="700"/></p>

---

### Analysis 4 — Claim Distribution by Severity

**Purpose:** Prioritize claim handling based on incident severity.

```sql
SELECT f.severity, COUNT(c.claim_Number) AS claim_count
FROM fnol f
JOIN claim c ON f.FNOL_id = c.FNOL_ID
GROUP BY f.severity
ORDER BY claim_count DESC;
```

<p align="center"><img src="Images/Picture4.png" alt="Analysis 4" width="700"/></p>

---

### Analysis 5 — Active Policies with Associated Claims

**Purpose:** Find active policies ordered by claim volume.

```sql
SELECT p.policyNumber, p.policyHolderName,
       COUNT(c.claim_Number) AS claim_count
FROM policy p
LEFT JOIN claim c ON p.policy_id = c.policy_id
WHERE p.active = 1
GROUP BY p.policyNumber, p.policyHolderName
ORDER BY claim_count DESC;
```

<p align="center"><img src="Images/Picture5.png" alt="Analysis 5" width="700"/></p>

---

### Analysis 6 — Inactive Policies with Unresolved Claims

**Purpose:** Flag inactive policies with still-open claims for follow-up.

```sql
SELECT p.policy_id, p.policyNumber,
       COUNT(c.claim_Number) AS claim_count
FROM policy p
JOIN claim c ON p.policy_id = c.policy_id
WHERE p.active = 0
GROUP BY p.policy_id, p.policyNumber
ORDER BY claim_count DESC;
```

<p align="center"><img src="Images/Picture6.png" alt="Analysis 6" width="700"/></p>

---

### Analysis 7 — Claims by Specific Policyholders

**Purpose:** Retrieve targeted claims and coverage for named policyholders.

```sql
SELECT c.claim_Number, p.policyHolderName,
       c.claimant_Fname, c.claimant_Lname, pc.coveredAmount
FROM claim c
JOIN policy p          ON c.policy_id          = p.policy_id
JOIN policycoverage pc ON p.policyCoverage_id  = pc.policyCoverage_id
WHERE p.policyHolderName IN ('John Doe', 'Jane Smith');
```

<p align="center"><img src="Images/Picture7.png" alt="Analysis 7" width="700"/></p>

---

### Analysis 8 — Most Used Payment Types

**Purpose:** Identify the most common payment methods for settlements.

```sql
SELECT paymentType, COUNT(financial_id) AS transaction_count
FROM financial
GROUP BY paymentType
ORDER BY transaction_count DESC
LIMIT 5;
```

<p align="center"><img src="Images/Picture8.png" alt="Analysis 8" width="700"/></p>

---

### Analysis 9 — Payment Trends by Status and Type

**Purpose:** Examine transaction volume and amounts by payment status.

```sql
SELECT f.paymentStatus, f.paymentType,
       COUNT(f.financial_id)                          AS total_transactions,
       SUM(CAST(f.paymentAmmount AS DECIMAL(10,2)))   AS total_amount
FROM financial f
GROUP BY f.paymentStatus, f.paymentType
ORDER BY f.paymentStatus, total_transactions DESC;
```

<!-- Add screenshot: images/analysis9.png -->
<p align="center"><img src="Images/Picture9.png" alt="Analysis 9" width="700"/></p>

---

### Analysis 10 — Monthly & Yearly Activity Trends

**Purpose:** Track claim activity volume over time for reporting.

```sql
SELECT YEAR(created)  AS activity_year,
       MONTH(created) AS activity_month,
       COUNT(history_id) AS total_activities
FROM history
WHERE created IS NOT NULL
GROUP BY activity_year, activity_month
ORDER BY activity_year DESC, activity_month DESC;
```

<p align="center"><img src="Images/Picture10.png" alt="Analysis 10" width="700"/></p>

---


<div align="center">
  <sub>INFO 5707.401 — Data Modelling for Information Professionals | University of North Texas</sub>
</div> 

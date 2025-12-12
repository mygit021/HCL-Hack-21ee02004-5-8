# HCL-Hack-21ee02004-5-8
This Repository is for HCL Hiring Hackathon 2025

# Insurance Data Model — Facts, Dimensions & ERD

## 🎯 Data Warehouse Design Objective
The source system contains information about:
- **Customers**
- **Policies**
- **Addresses**
- **Transactions**

To support analytics, reporting, and BI dashboards, we organize this data into **Fact** and **Dimension** tables.

---

# 📚 Dimension Tables

## 1. DIM_CUSTOMER
| Column Name |
|-------------|
| Customer_ID |
| Customer_Name |
| Customer_Segment |
| Marital_Status |
| Gender |
| DOB |
| Effective_Start_Dt |
| Effective_End_Dt |

**Reason:**  
Customer information is descriptive and does not change per transaction. It is used for segmentation and demographic analysis.

---

## 2. DIM_POLICY
| Column Name |
|-------------|
| Policy_Id |
| Policy_Name |
| Policy_Type_Id |
| Policy_Type |
| Policy_Type_Desc |
| Policy_Term |
| Policy_Start_Dt |
| Policy_End_Dt |

**Reason:**  
Policy definitions describe *what* was purchased. These values are relatively stable and serve as lookup fields.

---

## 3. DIM_ADDRESS
| Column Name |
|-------------|
| Address_ID *(Surrogate Key)* |
| Country |
| Region |
| State_or_Province |
| City |
| Postal_Code |

**Reason:**  
Geographical attributes group customers for regional performance, risk analysis, and reporting.

---

## 4. DIM_DATE
A standard date dimension supporting all date fields.

| Column Name |
|-------------|
| Date_Key *(YYYYMMDD)* |
| Full_Date |
| Day |
| Month |
| Month_Name |
| Quarter |
| Year |
| Day_of_Week |
| Is_Weekend |

**Reason:**  
A date dimension enables time-series analysis, quarter/year filtering, and comparing expected vs actual payments.

---

# 📊 Fact Table

## FACT_POLICY_TRANSACTION
| Column Name | Description |
|-------------|-------------|
| Customer_ID (FK) | Links to DIM_CUSTOMER |
| Policy_ID (FK) | Links to DIM_POLICY |
| Address_ID (FK) | Links to DIM_ADDRESS |
| Effective_Start_Dt (FK to DIM_DATE) |
| Effective_End_Dt (FK to DIM_DATE) |
| Next_Premium_Dt (FK to DIM_DATE) |
| Actual_Premium_Paid_Dt (FK to DIM_DATE) |
| Total_Policy_Amt | Total policy value |
| Premium_Amt | Premium for that policy |
| Premium_Amt_Paid_TillDate | Amount paid so far |

**Reason:**  
This table contains measurable, aggregatable values — premiums, payments, and amounts — making it the core fact table.

---

# 🧩 ERD Diagram (Markdown ASCII)

```text
                   +----------------------+
                   |     DIM_CUSTOMER     |
                   +----------------------+
                   | Customer_ID (PK)     |
                   | Name                 |
                   | Segment              |
                   | Marital_Status       |
                   | Gender               |
                   | DOB                  |
                   | Effective_Start_Dt   |
                   | Effective_End_Dt     |
                   +-----------+----------+
                               |
                               |
                               v
                    +---------------------------+
                    |   FACT_POLICY_TRANSACTION |
                    +---------------------------+
                    | Customer_ID (FK)          |
                    | Policy_ID (FK)            |
                    | Address_ID (FK)           |
                    | Effective_Start_Dt (FK)   |
                    | Effective_End_Dt (FK)     |
                    | Next_Premium_Dt (FK)      |
                    | Actual_Premium_Paid_Dt(FK)|
                    | Total_Policy_Amt          |
                    | Premium_Amt               |
                    | Premium_Paid_TillDate     |
                    +-----------+-----------+---+
                                |           |
                +---------------+           +------------------+
                |                                        |
                v                                        v
      +-------------------+                     +--------------------+
      |    DIM_POLICY     |                     |    DIM_ADDRESS     |
      +-------------------+                     +--------------------+
      | Policy_ID (PK)    |                     | Address_ID (PK)    |
      | Policy_Name       |                     | Country            |
      | Policy_Type_ID    |                     | Region             |
      | Policy_Type       |                     | State/Province     |
      | Policy_Type_Desc  |                     | City               |
      | Policy_Term       |                     | Postal_Code        |
      | Start_Date        |                     +--------------------+
      | End_Date          |
      +-------------------+

                
                             +----------------------+
                             |      DIM_DATE        |
                             +----------------------+
                             | Date_Key (PK)        |
                             | Day                  |
                             | Month                |
                             | Quarter              |
                             | Year                 |
                             +----------------------+

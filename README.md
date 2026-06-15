# 🚨🏥 Code Blue - Emergency Operations & Patient Flow Analytics (Power BI)

This project is a comprehensive healthcare analytics solution designed to monitor and optimize emergency department operations and patient flow across hospital networks. The system tracks key metrics including patient visit volumes, admission types, wait times, triage efficiency, treatment timelines, ICU utilization, readmission rates, and clinical outcomes (severity levels, mortality). It provides real-time visibility into operational performance by hospital, department, region, and clinical indicators, enabling healthcare administrators and clinicians to identify bottlenecks, improve patient outcomes, and optimize resource allocation across the emergency care continuum.

This project transforms transactional emergency care data into an interactive business intelligence dashboard that helps healthcare leaders identify **where operational bottlenecks exist, how patient flow impacts outcomes, and where resource optimization can save lives.**

🔗 **Live Report:**
🔗 **[View Live Report](https://app.powerbi.com/view?r=eyJrIjoiMDU2NzAyNGEtZmMxMS00YWIxLTk4YTUtYzUzOTlhZjZhYTcwIiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)** &nbsp;|&nbsp; 📃 **[Dataset](Datasets)** &nbsp;|&nbsp; 📖 **[Medium Article](https://medium.com/@kudehinbusamad/analysing-last-mile-delivery-performance-across-12-lagos-zones-a-sql-power-bi-case-study-2c3b8805b20e)** &nbsp;|&nbsp; 👤 **[LinkedIn](https://www.linkedin.com/in/abdulsamad-kudehinbu/)** &nbsp;|&nbsp; 🌐 **[Portfolio](https://sites.google.com/view/abdulsamadportfolio/home)**

---
---

# 📌 Project Overview

Emergency departments across healthcare networks generate massive volumes of operational and clinical data daily. Without proper visualization and analysis, identifying inefficiencies, capacity constraints, and outcome drivers can be impossible.

This Power BI report was developed to provide insights into:
- 🏥 Patient arrival patterns and triage demand
- ⏱️ Waiting times and treatment delays
- 🛏️ Bed capacity utilization and ICU demand
- 👨‍⚕️ Staff workload and resource allocation
- 📍 Performance variation across hospital locations
- 🔄 Patient discharge movement and readmissions
- 💔 Clinical outcomes and mortality trends
- 💰 Operational costs and resource efficiency

The report enables **data-driven decision-making for emergency care operations, patient experience improvement, and resource planning.**

---

# 🎯 Business Problem

Hospital networks struggle to answer critical operational questions:
- **How are patient volumes and pressure distributed across our hospitals?**
- **Where do the biggest bottlenecks exist in our emergency care pathway?**
- **Which hospitals or regions show the strongest or weakest performance?**
- **How efficiently are we using our bed capacity and ICU resources?**
- **What factors are driving poor patient outcomes like readmissions and mortality?**
- **Where can we reallocate staff and resources for maximum impact?**
- **Are there concerning trends or outliers we need to address immediately?**

Without a centralized analytics dashboard, these insights remain buried in separate systems and disconnected spreadsheets.

This dashboard solves the problem by consolidating all operational and clinical metrics into an **interactive Power BI report** that tells the story of emergency care performance.

---

# 📂 Dataset

The dataset represents **operational and clinical data from an NHS emergency care network** operating across multiple hospital locations and regions.

### 📊 Dataset Scope
- Multi-hospital NHS trust network
- Multiple hospital locations by region
- Patient visit transactions with clinical outcomes
- Operational metrics (beds, ICU capacity, staff)
- Time period: **Historical operational baseline**
- **32,639+ populated rows across 11 sheets**

### 📑 Key Dimension Tables

| Table | Purpose | Key Fields |
|-------|---------|-----------|
| **Dim_Hospital** | Hospital reference data | Hospital_ID, Hospital_Name, Region, Beds, ICU_Beds, ED_Bays, Staff_FTE, Annual_Budget |
| **Dim_Department** | Clinical departments | Department_ID, Department_Name |
| **Dim_Patient** | Patient demographics | Patient_ID, Age, Insurance_Type |
| **Dim_Doctor** | Clinical staff | Doctor_ID, Doctor_Name, Specialty |
| **Dim_Date** | Time dimension | Full_Date, Month, Year, Day_of_Week |
| **Dim_Diagnosis** | Clinical classifications | Diagnosis_Category, ICD_Code |
| **Dim_Region** | Geographic hierarchy | Region_ID, Region_Name, City |

### 📑 Key Fact Tables

| Table | Purpose | Key Metrics |
|-------|---------|-----------|
| **Fact_Patient_Visits** | Emergency visit transactions | Arrival_DateTime, Wait_Time_Minutes, Treatment_Delay_Minutes, Length_of_Stay_Hours, Severity_Level, Outcome, Mortality_Flag, Readmission_30_Days_Flag, Revenue_Amount, Treatment_Cost |
| **Fact_Staffing** | Staff capacity tracking | Staff_FTE, Shifts_Scheduled |
| **Fact_Financials** | Operational costs | Treatment_Cost, Revenue_Amount, Margin |

### 📈 Key Metrics and KPIs

Core measures tracked:
- **Total Patient Visits** = COUNTA(Fact_Patient_Visits[Visit_ID])
- **Avg Wait Time** = AVERAGE(Fact_Patient_Visits[Wait_Time_Minutes])
- **Avg Severity Score** = AVERAGE(Fact_Patient_Visits[Severity_Level])
- **ICU Admission Rate** = DIVIDE(CALCULATE(COUNTROWS(Fact_Patient_Visits), Fact_Patient_Visits[ICU_Required_Flag] = TRUE()), COUNTA(Fact_Patient_Visits[Visit_ID])) * 100
- **30-Day Readmission Rate** = DIVIDE(CALCULATE(COUNTROWS(Fact_Patient_Visits), Fact_Patient_Visits[Readmission_30_Days_Flag] = TRUE()), COUNTA(Fact_Patient_Visits[Visit_ID])) * 100
- **Bed Occupancy Rate** = DIVIDE(SUMPRODUCT(Fact_Patient_Visits[Length_of_Stay_Hours] / 24), Total_Beds) * 100
- **Total Triaged Patients** = COUNTA(Fact_Patient_Visits[Triage_DateTime])
- **Total Treatment Started Patients** = COUNTA(Fact_Patient_Visits[Treatment_Start_DateTime])

---

# 🧹 Data Cleaning & Preparation

Data preparation was completed using **Power Query and DAX**.

Key steps included:
- Validating datetime sequences (Arrival → Triage → Treatment Start → Discharge)
- Calculating derived metrics (wait times, treatment delays, length of stay)
- Standardizing location and department hierarchies
- Ensuring referential integrity across fact and dimension tables
- Creating time intelligence calendars for trend analysis
- Identifying and flagging data quality issues

The final dataset was optimized for **fast dashboard performance and multi-dimensional drill-down analysis.**

---

# 🧠 Data Model
The data model follows a **Galaxy Schema (Fact Constellation Schema)**, consisting of multiple fact tables sharing conformed dimension tables.
<img width="912" height="667" alt="model" src="https://github.com/user-attachments/assets/33659b80-00b9-47c6-8315-2dbf22fc396d" />

### 🔗 Relationships
* **Conformed Dimensions:** Tables like `Dim_Hospital`, `Dim_Department`, and `Dim_Date` act as conformed dimensions, connecting to multiple fact tables to allow cross-functional analysis.
* **One-to-Many Relationships:** Standard data warehouse relationships ($1 \rightarrow *$) are implemented, ensuring filters flow correctly from the dimensions down to the fact tables.
* **Geographic Snowflake:** `Dim_Hospital` connects out to `Dim_Region`, establishing a normalized geographic hierarchy for regional drill-down.

This robust multi-fact model enables **cross-subject area analysis (e.g., correlating staffing burnout against financial costs or patient outcomes), efficient filtering, and high-performance dashboard queries.**
---

# 📊 Dashboard Structure

The report consists of **three analytical pages**, each filterable by Year, Month, Region, and Admission Type, designed for different healthcare leadership personas.

---

# 📈 Page 1 — Executive Overview

### 🎯 Purpose
*"An executive overview of patient volume, operational pressure, clinical outcomes, and financial performance across the Code Blue hospital network."*
<img width="1397" height="772" alt="Page 1" src="https://github.com/user-attachments/assets/41a9bfe1-08ff-4959-9fd8-d92ce4efdb07" />


### 📌 KPIs
- 🚑 **Patient Visits** — 409 (▲ 5 vs Previous Month)
- ⏱️ **Avg Wait Time (Mins)** — 62 (▼ 1.02 vs Previous Month)
- 🛏️ **Bed Occupancy Rate** — 64% (▲ 12.78% vs Previous Month)
- 💔 **Mortality Rate** — 3.22% (▲ 0.09% vs Previous Month)
- 😊 **Avg Satisfaction Score** — 62.35 (▼ 0.93 vs Previous Month)
- 💰 **Avg Profit Margin** (toggleable KPI)

### 📊 Visuals
- **Monthly Trends** — A field-parameter-driven bar chart letting users switch between Patient Visits, Avg Wait Time, Bed Occupancy Rate, Mortality Rate, and Avg Profit Margin across all 12 months
- **Patient Visits by Hospital** — Ranked bar chart of all 12 hospitals in the network, from Sandwell General (41 visits) to Alder Hey Children's (33 visits)
- **Patient Visits by Location** — UK map visualization showing regional distribution of visits (London region leading at 165, followed by clusters of 144, 115, 114, 97, 41, 37, and 36)
- **Patient Visits by Admission Type** — Donut chart showing Emergency (40.34%), Urgent (27.87%), Elective (23.72%), and Transfer (8.07%)

---

# 🏥 Page 2 — Clinical Performance

### 🎯 Purpose
*"A detailed view of patient flow, triage performance, clinical outcomes, and care quality across the Code Blue hospital network."*
<img width="1397" height="777" alt="Page 2" src="https://github.com/user-attachments/assets/8bf56f47-4ef3-4a27-ac0d-b3507196854f" />


### 📌 KPIs
- 🛏️ **Avg Length of Stay (Hrs)** — 44 (▲ 2 vs Previous Month)
- ⏱️ **Avg Treatment Delay (Mins)** — 74 (▲ 4 vs Previous Month)
- 🆘 **ICU Admission Rate** — 16.38% (▲ 1.53% vs Previous Month)
- 🔄 **30-Day Readmission Rate** — 17.11% (▲ 2.26% vs Previous Month)
- 🔥 **Avg Severity Score** — 3.45 (▲ 0.05 vs Previous Month)

### 📊 Visuals
- **Avg Length of Stay (Hrs) by Diagnosis Category** — Toggleable with ICU Admissions; ranks all 13 diagnosis categories from Cardiovascular (69 hrs) down to Obstetric (27 hrs)
- **Total Patient Visits by Severity Level** — Bar chart across severity levels 1–5, peaking at Level 3 (133 visits) and Level 4 (122 visits)
- **Patient Flow Funnel** — Tracks patient drop-off from Admitted (409) → Triaged (395) → Treatment Started (388) → Discharged (373), an overall conversion of 91.2%
- **Hospital Performance Intelligence Table** — Sortable table comparing all 12 hospitals on ICU Admissions, 30-Day Readmission Rate, Avg Length of Stay, Avg Satisfaction Score, and Avg Severity Score

---

# 👥 Page 3 — Workforce and Financial Health

### 🎯 Purpose
*"A comprehensive view of workforce pressure, staffing efficiency, and financial performance."*
<img width="1397" height="777" alt="Page 3" src="https://github.com/user-attachments/assets/d8a2354a-89de-464a-a721-cd026a942ae2" />


### 📌 KPIs
- 💰 **Revenue** — $454.32M (▲ $39.08M vs Previous Month)
- 💸 **Operational Cost** — $337.01M (▲ $23.07M vs Previous Month)
- 📊 **Profit Margin** — 18.52% (▲ 0.34% vs Previous Month)
- 🔥 **Burnout Risk Index** — 0.62 (▲ 0.01 vs Previous Month)
- 🤒 **Staff Absences** — 406 (▲ 102 vs Previous Month)

### 📊 Visuals
- **Revenue by Month** — Toggleable bar chart switching between Revenue, Operational Cost, Profit Margin, Burnout Risk Index, and Staff Absences across all 12 months
- **Doctors Intelligence Table** — Sortable table of clinical staff by Specialty, Grade, Patients Seen, Avg Wait Time, and Avg Satisfaction Score
- **Revenue vs Operational Cost by Hospital** — Toggleable with Burnout Risk; horizontal bar chart comparing revenue and operational cost across all 12 hospitals, with Sandwell General and Leeds General showing the widest revenue-cost spread

---

# 💡 Key Insights & Business Question Analysis (December 2025)

### **How does patient volume change over time, and what trend patterns should leaders pay attention to?**
December's 409 patient visits represent a **slight uptick of 5 visits** month-over-month, but the bigger story is in the monthly trend chart: Q1 (Jan–Mar) recorded visit volumes nearly **40–50% higher** than the rest of the year, after which volumes settled into a lower, relatively stable band from April through December. Revenue follows an almost identical seasonal shape — peaking above $550M in Q1 and stabilizing near $400–450M for the remainder of the year. This suggests a strong **early-year demand surge** that the network should plan capacity and staffing around well in advance each year.

### **Which hospitals account for the highest activity, pressure, or performance variation?**
**Sandwell General Hospital** and **Spire Manchester Hospital** lead the network in patient visits (41 and 40 respectively), but the Hospital Performance Intelligence Table reveals a critical disconnect: **Sandwell General** also has the **highest 30-day readmission rate in the network at 31.71%**, paired with the longest average length of stay (77 hrs) and one of the lowest satisfaction scores (61.51). Meanwhile, **King's College Hospital NHS FT** posts a high ICU admission count (8) alongside the second-highest readmission rate (25.64%) but the shortest length of stay (24 hrs) — a pattern worth investigating, as short stays combined with high readmissions can signal premature discharge. By contrast, **University College London Hospitals** combines a moderate readmission rate (5.26%) with a strong satisfaction score (59.07) and high ICU throughput, suggesting more effective discharge protocols.

### **Where are the biggest gaps, risks, or inefficiencies in the process?**
The **Patient Flow Funnel** is the clearest red flag this month: of 409 admitted patients, only **373 were discharged — an 8.8% drop-off** across the care journey. The steepest single drop occurs between Treatment Started (388) and Discharged (373), a loss of 15 patients that likely reflects transfers, ongoing care, or data lag, but still warrants review. Combined with a **74-minute average treatment delay (up 4 minutes)** and a **62-minute average wait time**, patients are spending substantial time before active care begins. On the workforce side, **staff absences jumped by 102 to 406** this month — a sharp spike that, paired with a rising **Burnout Risk Index of 0.62**, points to a workforce strain that could compound wait-time and treatment-delay issues if left unaddressed.

### **How do key metrics compare across important business segments?**
- **By severity level:** Visits are concentrated in moderate-to-high severity bands — Level 3 (133) and Level 4 (122) together account for over 60% of all visits, while Level 1 (16) is rare. This skew toward higher-acuity cases helps explain the elevated 44-hour average length of stay and 16.38% ICU admission rate.
- **By admission type:** Emergency admissions dominate at 40.34%, followed by Urgent (27.87%), Elective (23.72%), and Transfer (8.07%) — confirming this is primarily an unplanned-care-driven network.
- **By diagnosis category:** Cardiovascular cases drive the longest average stays (69 hrs), nearly **2.5x longer** than Obstetric cases (27 hrs), indicating cardiovascular patients are consuming a disproportionate share of bed capacity.
- **By region:** Patient visits are heavily concentrated in the London area (165 visits), more than the next three regions combined — a geographic concentration with direct implications for staffing and bed allocation in that region.

### **Which factors appear most connected to better or worse outcomes?**
This month's data reinforces several outcome drivers:
- **Length of stay and readmissions move together at the extremes** — Sandwell General's combination of the longest stay (77 hrs) and highest readmission rate (31.71%) suggests prolonged stays aren't necessarily translating into better discharge outcomes.
- **High patient volume doesn't guarantee high satisfaction** — despite ranking #1 in visits, Sandwell General's satisfaction score (61.51) sits below the network's overall average satisfaction (62.35), while lower-volume hospitals like Norfolk & Norwich (66.34) and Bristol Royal Infirmary (66.24) post the strongest satisfaction scores.
- **Rising treatment delays correlate with the mortality rate increase** — the 4-minute rise in treatment delay and the 0.09% rise in mortality rate moved in the same direction this month, a relationship worth monitoring over subsequent months.
- **Staff absences and burnout risk are both trending upward together** — a 102-person spike in absences alongside a 0.01 increase in Burnout Risk Index suggests workforce capacity strain may be an early driver of the wait-time and treatment-delay pressures seen elsewhere in the dashboard.

### **Where are the strongest opportunities for operational improvement?**
**Priority improvement areas based on December 2025 data:**
1. **Investigate Sandwell General's discharge protocols** — its combination of highest volume, longest stay, and highest readmission rate makes it the single biggest opportunity for standardization gains.
2. **Address the rising treatment delay (74 mins, ▲4)** — even a modest reduction here could help reverse the small mortality rate uptick.
3. **Tackle the staff absence spike (406, ▲102)** — with Burnout Risk Index also climbing, proactive workforce support could prevent further deterioration in wait times and care quality.
4. **Plan for Q1 demand surges** — given the sharp visit and revenue spike in Jan–Mar relative to the rest of the year, pre-positioning staff and bed capacity ahead of the next Q1 cycle could reduce strain before it occurs.
5. **Review cardiovascular care pathways** — as the diagnosis category with by far the longest average length of stay (69 hrs), even small efficiency gains here would free meaningful bed capacity network-wide.
6. **Replicate King's College Hospital's short-stay model carefully** — while its 24-hour average length of stay is the shortest in the network, its high readmission rate (25.64%) suggests this efficiency should be paired with stronger post-discharge follow-up before being held up as a benchmark.

---

# 🚀 Strategic Recommendations

### 📍 **Standardize Discharge Protocols at High-Readmission Hospitals**
Focus first on Sandwell General Hospital (31.71% readmission rate, 77-hr average stay) and King's College Hospital NHS FT (25.64% readmission rate). Document and adapt the discharge approaches used at University College London Hospitals (5.26% readmission rate) network-wide.

### 🛏️ **Implement Dynamic Bed Management for Cardiovascular Pathways**
With cardiovascular cases averaging 69 hours — the longest of any diagnosis category — targeted care pathway reviews and earlier step-down planning could meaningfully reduce bed occupancy pressure (currently at 64%, up nearly 13 points month-over-month).

### 👥 **Respond to the Staff Absence and Burnout Spike**
A jump of 102 staff absences alongside a rising Burnout Risk Index (0.62) signals workforce strain. Prioritize coverage planning and wellbeing support, especially at high-volume, high-pressure hospitals like Sandwell General and Spire Manchester.

### 🚨 **Build a Q1 Surge Readiness Plan**
The data shows visit volumes and revenue both spike sharply in Jan–Mar relative to the rest of the year. Use this pattern to pre-position staffing, beds, and ICU capacity ahead of the next high-demand period.

### 📊 **Establish KPI Benchmarks Using Top Performers**
Use Norfolk & Norwich University Hospital and Bristol Royal Infirmary (highest satisfaction scores) and University College London Hospitals (lowest readmission rate among high-ICU hospitals) as internal benchmarks for satisfaction and discharge quality targets.

### 🔄 **Tighten the Patient Flow Funnel**
With an 8.8% overall drop-off from admission to discharge and a 74-minute average treatment delay, map the full Admitted → Triaged → Treatment Started → Discharged journey to identify and close the gaps contributing to both delay and drop-off.

---

# 🔚 Conclusion

This emergency operations dashboard transformed raw operational and clinical data into a strategic tool for healthcare leaders. For December 2025, the data tells a clear story: patient volumes and revenue have settled into a stable post-Q1 baseline, but rising readmissions, treatment delays, staff absences, and burnout risk signal mounting operational pressure beneath the surface. Instead of wondering where bottlenecks exist or why some hospitals outperform others, stakeholders now see it clearly: Sandwell General needs discharge protocol attention, cardiovascular care pathways need efficiency reviews, and workforce strain needs proactive management before it compounds further. For NHS trusts and hospital networks managing complex emergency care operations across multiple locations, this kind of analysis is essential — transforming the question from "Are we meeting our obligations?" to "How can we deliver better care, more efficiently?"

---

# ✨ Key Dashboard Features

- ✅ Interactive slicers and cross-filters for dynamic exploration (Year, Month, Region, Admission Type)
- ✅ Geographic mapping for regional performance analysis
- ✅ Multi-dimensional drill-down from network → hospital → department → individual metrics
- ✅ Trend analysis across all 12 months
- ✅ KPI field parameters for flexible metric selection (toggleable charts on each page)
- ✅ Clinical outcome tracking (readmissions, mortality, severity)
- ✅ Operational efficiency metrics (wait times, treatment delays, bed occupancy)
- ✅ Financial and workforce health monitoring (revenue, costs, burnout risk, absences)

---

# 🔗 Links

- 📊 [View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiMDU2NzAyNGEtZmMxMS00YWIxLTk4YTUtYzUzOTlhZjZhYTcwIiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)
- 👤 [Connect on LinkedIn](https://www.linkedin.com/in/abdulsamad-kudehinbu/)
- 📝 [View My Portfolio](https://sites.google.com/view/abdulsamadportfolio/home)

---

**Last Updated:** June 2026
**Report Status:** Active & Maintained

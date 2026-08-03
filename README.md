# project-da-hr-analyst
# 📊 Synapse Solutions HR Analytics: Unveiling Workforce Dynamics & Optimizing HR Strategies

## I. Context
*   **Overall Objective:** Identify and address existing issues within Synapse Solutions' Human Resources (HR) data to drive data-driven strategic decisions.
*   **Specific Goal:** Evaluate the effectiveness of recruitment processes, assess the quality of employee engagement and well-being, and measure overall employee performance within the company.
*   **Context / Problem Statement (5W1H):**
    *   *Why is there a problem:* Synapse Solutions currently lacks a comprehensive understanding of its HR landscape, making it difficult to pinpoint pain points, optimize operational efficiencies, and proactively address challenges like employee turnover or sub-optimal recruitment.
    *   *Where / When:* The analysis is focused on internal HR data from Synapse Solutions, covering approximately 1000 employees. The dataset spans from 1971 to 2023, representing simulated historical and current workforce information. The analysis is performed using Python scripts.
*   **Stakeholders & Roles (Who):**
    *   *Data Analyst (You):* Performed data ingestion, cleaning, transformation, and comprehensive analysis, leveraging strong HR domain knowledge to derive meaningful insights and answer key business questions.
    *   *HR Department (End-users):* Will utilize the analytical dashboard and findings to identify critical HR issues, propose actionable solutions for optimizing recruitment costs, improve retention strategies for high-performing employees, and conduct more holistic employee performance evaluations. Insights will also inform future data collection improvements.
    *   *Board of Directors (Strategic Stakeholders):* Will review the summarized findings to approve optimized HR budgets, define future recruitment directions, and establish appropriate benefits and welfare policies tailored to diverse employee segments.

## II. Domain Knowledge & Terminology
*   **Employee Turnover:** The rate at which employees leave an organization. This project distinguishes between `Voluntary` (e.g., career change, better opportunity) and `Involuntary` (e.g., company downsizing) turnover, and identifies `root_cause_leaving`.
*   **Recruitment Effectiveness:** The measure of how successful various `Source of Hire` channels are in attracting, hiring, and retaining suitable candidates. Key metrics include `Starting Salary` and subsequent `Performance Review Score`.
*   **Work-Life Balance Score:** An internal rating (typically 1-5, where 5 is best) reflecting employees' perceived balance between work demands and personal life, often correlated with satisfaction and retention.
*   **Absenteeism Rate:** Categorical measure (e.g., Low, Moderate, High) indicating the frequency or duration of employee absences, which can signal issues with well-being, engagement, or workload.
*   **Performance Review Score:** An ordinal rating (e.g., Exceptional, Outstanding, Average, Below Expectations) used to evaluate individual employee performance against set objectives.
*   **Salary Growth (%):** A calculated metric showing the percentage increase in an employee's `Current Salary` compared to their `Starting Salary`, crucial for assessing compensation competitiveness and career progression.

## III. Main Content

### 1. Data Overview & Analytical Objectives
*   **Dataset:** This project utilizes a simulated HR dataset from Synapse Solutions, encompassing records for 1000 employees from 1971 to 2023. The data is initially structured across five separate tables: `Employee`, `Hiring`, `Performance`, `Compensation`, and `Exit_Interview`, which are later merged for a holistic view.
*   **Objectives:** Answer the following key questions:
    1.  What are the primary reasons employees leave the company, and is there evidence of "brain drain" (loss of high-performing individuals)?
    2.  Which recruitment sources (`Source of Hire`) are most effective in attracting and retaining employees with good performance?
    3.  How is the overall HR landscape characterized in terms of demographics, tenure, work-life balance, and absenteeism?
    4.  How does employee performance (`Performance Review Score`) correlate with factors like `Training Courses Attended`, `Salary Growth (%)`, and `Promoted` status?
    5.  Are there any significant compensation-related issues, such as negative `Salary Growth (%)`, that might impact employee satisfaction and retention?

### 2. Data Dictionary
The following are key columns from the unified HR dataset (derived from merging original tables like Employee, Hiring, Performance, Compensation, and Exit_Interview):

| **Column Name** | **Description** | **Data Type** | **Encoding / Notes** |
| :---------------------- | :------------------------------------------------------ | :------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Employee_ID` | Unique identifier for each employee. | `Integer` | Primary Key. |
| `Name` | Full name of the employee. | `String` | |
| `email` | Employee's email address. | `String` | |
| `gender` | Employee's gender. | `String (Category)` | E.g., Male, Female. |
| `Department` | Department where the employee works. | `String (Category)` | E.g., Engineering, Human Resources, Sales. |
| `Education level` | Highest education level attained by the employee. | `String (Category)` | E.g., PhD, Master's Degree, Bachelor's Degree, High School Diploma. |
| `Date Hired` | Date when the employee was hired. | `Date` | Used to calculate `Tenure`. |
| `Still Employed` | Boolean indicating if the employee is currently active. | `Boolean` | `True` for active, `False` for exited. |
| `Work-Life Balance Score` | Internal score for work-life balance. | `Integer` | Scale from 1 (poor) to 5 (excellent). |
| `Absenteeism Rate` | Categorical rate of employee absenteeism. | `String (Category)` | E.g., Low, Moderate, High. |
| `Performance Review Score` | Employee's performance review rating. | `String (Ordinal)` | E.g., Exceptional, Outstanding, Superior, Good, Average, Below Expectations, Needs Improvement. |
| `Training Courses Attended` | List of training courses completed by the employee. | `String` | Can be multiple courses separated by commas. |
| `Current Salary` | Employee's current annual salary. | `Float` | Values must be > 0. (Currency unit assumed to be USD). |
| `Benefits` | Key benefits received by the employee. | `String` | E.g., performance bonuses, gym membership, remote work option. |
| `Source of Hire` | Channel through which the employee was recruited. | `String (Category)` | E.g., Social media, Recruiter, Job board, Employee referral. |
| `Starting Salary` | Employee's salary upon joining the company. | `Float` | Used to calculate `Salary Growth (%)`. |
| `Salary Growth (%)` | Percentage increase from `Starting Salary` to `Current Salary`. | `Float` | Calculated as `((Current Salary - Starting Salary) / Starting Salary) * 100`. Note: Negative values indicate salary reduction. |
| `Promoted` | Indicates if the employee has been promoted. | `String (Boolean)` | 'Yes' or 'No'. |
| `reason_for_leaving` | Specific reason cited by the employee for their departure. | `String` | E.g., Career change, Health reasons, Company downsizing. |
| `last_day_of_employment` | The employee's last day of work. | `Date` | Relevant for `Still Employed = False`. |
| `root_cause_leaving` | Categorized root cause for employee departure. | `String (Category)` | E.g., Career & Compensation, Personal - Life Events, Organizational Restructuring. |
| `turnover_type` | Classification of turnover. | `String (Category)` | E.g., Voluntary, Involuntary. |
| `Performance_group` | Categorical grouping of `Performance Review Score`. | `String (Category)` | E.g., High, Meet Expectation, Low. |

### 3. Feature Engineering based on Domain Knowledge
*   **`Tenure (Years)`:** Created by calculating the difference in years between `Date Hired` and either the current date (for active employees) or `last_day_of_employment` (for exited employees). *Reason:* Tenure is a crucial metric for understanding employee loyalty, experience levels, and potential correlations with performance, work-life balance, and turnover risk.
*   **`Performance_group`:** Derived from mapping the granular `Performance Review Score` into broader, more actionable categories such as 'High', 'Meet Expectation', and 'Low'. *Reason:* Grouping performance scores simplifies high-level analysis and visualization, making it easier for stakeholders to grasp overall performance trends and identify segments for targeted interventions.
*   **`Salary Growth (%)`:** This feature was explicitly calculated as `((Current Salary - Starting Salary) / Starting Salary) * 100`. *Reason:* It provides a direct, normalized measure of an employee's financial progression within the company, indicating the impact of promotions, raises, and potential salary adjustments. Analyzing this helps assess compensation fairness and its role in employee satisfaction and retention.

### 4. Analytics Workflow

#### Step 1: Data Loading & Pre-processing
*   **Tools:** Python (`pandas`, `io`, `google.colab.files`)
*   **Details:**
    *   Initial data ingestion involved loading five separate Excel sheets/CSV files (`Employee`, `Hiring`, `Performance`, `Compenstaion`, `Exit_Interview`) into pandas DataFrames.
    *   Conversion of relevant columns to appropriate data types, specifically `Date Hired` to datetime objects (`pd.to_datetime`).
    *   Addressed potential formatting issues, null/missing values, and basic data inconsistencies as a foundational step, ensuring data integrity.
    *   The multiple datasets were then merged (implicitly leading to `Updated_HR_Analytics.csv`) to create a unified analytical dataset, linking records via `Employee_ID`.

#### Step 2: Exploratory Data Analysis (EDA)
*   **Tools:** Python (`pandas`, `numpy` for consistency checks)
*   **Details:**
    *   Conducted initial descriptive statistics (`.describe()`, `.info()`) for both numerical and categorical columns to understand data distributions, ranges, and unique values across all merged tables.
    *   Identified potential data anomalies, such as negative `Salary Growth (%)` values, which were flagged for further investigation or rectification.
    *   Performed cross-table consistency checks (e.g., between `df_Hire` and `df_Comp` for `Source of Hire` and `Starting Salary`) to ensure data integrity across merged datasets.
    *   Analyzed the presence and types of missing values to inform imputation or dropping strategies in subsequent steps.

#### Step 3: Data Modeling & Transformation
*   **Tools:** Python (`pandas`) (SQL capabilities were acknowledged but not explicitly used in the provided snippet)
*   **Details & Techniques:**
    *   **Data Integration:** Merged the individual HR data tables (`df_Employ`, `df_Hire`, `df_Perf`, `df_Comp`, `df_Exit`) into a single, comprehensive dataset. This involved identifying common keys (`Employee_ID`) and handling potential conflicts or redundant columns (e.g., `Source of Hire` and `Starting Salary` appearing in both `Hiring` and `Compensation` tables, which were checked for consistency).
    *   **Feature Creation:** Engineered new, derived features essential for HR analysis, such as `Tenure (Years)` and `Performance_group`, as detailed in Section III.3.
    *   **Data Cleaning Refinement:** Further refined data by addressing specific issues like the negative `Salary Growth (%)` by analyzing its frequency and potential root causes (e.g., demotions, data entry errors).

#### Step 4: Data Visualization
*   **Tools:** Power BI (as implied by end-user requirements for a dashboard)
*   **Details:**
    *   **Layout Strategy:** Dashboard design will follow a logical flow from general overview to specific details, typically arranged from left to right and top to bottom, illustrating cause-and-effect relationships where applicable (e.g., linking low work-life balance to higher turnover).
    *   **Chart Selection:**
        *   **Bar Charts:** Utilized for categorical distributions (e.g., `Department` breakdown, `Education level` distribution, `Source of Hire` effectiveness, `Absenteeism Rate` distribution).
        *   **Pie/Donut Charts:** Employed for part-to-whole relationships (e.g., `gender` distribution, `turnover_type` proportions, distribution of `root_cause_leaving`).
        *   **Line Charts:** Used to display trends over time (e.g., `Date Hired` over years, `Salary Growth (%)` trends by tenure).
        *   **Scatter Plots/Bubble Charts:** To visualize relationships and correlations between numerical variables (e.g., `Tenure` vs. `Current Salary`, `Work-Life Balance Score` vs. `Absenteeism Rate`).
        *   **Gauge Charts/KPIs:** For critical summary metrics like overall turnover rate, average `Work-Life Balance Score`, and average `Salary Growth (%)`.
        *   **Table Visuals:** To display detailed `Data Dictionary` or drill-down information.
        *   **Link Dashboard:** https://app.powerbi.com/reportEmbed?reportId=c094eeb9-fc66-49d4-9820-d31f699eff91&autoAuth=true&ctid=f7568d95-5bfd-4236-9bf4-56ef3e8c2466
        *   <img width="1444" height="812" alt="image" src="https://github.com/user-attachments/assets/e3579a39-02e5-4a05-a967-14a504f0d8f9" />
        *   <img width="1445" height="811" alt="image" src="https://github.com/user-attachments/assets/03a8f7f2-f32f-4f7a-a486-1008460fbe4f" />

### 5. Conclusions & Actionable Recommendations
*   **Key Findings:**
    *   Identification of dominant `reason_for_leaving` (e.g., "Career change", "Company downsizing") and `root_cause_leaving` (e.g., "Career & Compensation", "Personal - Life Events"), highlighting areas for targeted retention efforts.
    *   Analysis of `Source of Hire` effectiveness, revealing which channels yield more engaged or higher-performing employees.
    *   Insights into `Work-Life Balance Score` and `Absenteeism Rate` by department or tenure, pointing to specific areas for employee well-being initiatives.
    *   Correlation analysis between `Performance Review Score`, `Training Courses Attended`, and `Promoted` status, indicating potential gaps or successes in employee development.
    *   Identification of significant negative `Salary Growth (%)` instances, which may indicate issues with compensation structures, demotions, or data errors requiring immediate attention.
*   **Actionable Recommendations:**
    *   **For HR Department:**
        *   Develop tailored retention programs based on identified `root_cause_leaving` insights, focusing on competitive compensation and career development for high-performing employees.
        *   Optimize recruitment budgets by prioritizing `Source of Hire` channels proven to deliver higher quality and longer-tenured talent.
        *   Implement wellness programs or re-evaluate workloads in departments showing low `Work-Life Balance Score` or high `Absenteeism Rate`.
        *   Review and refine existing performance evaluation criteria and training programs based on `Performance_group` analysis.
        *   Investigate and rectify data quality issues, particularly concerning negative `Salary Growth (%)`, to ensure accurate compensation analysis.
    *   **For Board of Directors:**
        *   Allocate budget towards strategic recruitment initiatives for high-performing `Source of Hire` channels.
        *   Approve new or revised benefits and welfare policies that directly address employee satisfaction drivers and mitigate key `reason_for_leaving`.
        *   Support initiatives aimed at fostering employee engagement and work-life balance, recognizing their impact on overall organizational health and productivity.

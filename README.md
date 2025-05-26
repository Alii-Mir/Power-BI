- The Power BI file `PowerBI.pbix` is available on the project main page.

- There is also a link to a short video for the full project presentation: [Project Overview Presentation](https://drive.google.com/file/d/1H7lKETwM7dhWyTVBouzA3vWHaI3zIuHj/view)

- We have done a full analysis (4 major analyses) through Python implementations.
___

# 🌿 Business Data Analysis Dashboard (Power BI)

This Power BI project was developed for a landscaping company to help identify areas for operational improvement. The company provided data on jobs, employees, and job assignments through three CSV files:

- `landscaping.csv`: Job details (type, request/start/completion dates, customer satisfaction, invoice, etc.)
- `employees.csv`: Employee IDs and hourly wages
- `calendar.csv`: Daily job assignments per employee

Our goal was to answer the high-level question: **"How can our company be improved?"**

To do this, we focused on two key sub-goals:
1. **Analyzing employee performance and productivity**
2. **Optimizing job scheduling and resource allocation**

---

## ✅ Sub-Goal 1: Employee Performance & Productivity

### 🎯 Objective
Understand how well each employee performs across different job types by analyzing:
- **Mean customer satisfaction**
- **Total number of days worked**

This helps identify which job types an employee excels at or struggles with, and supports better staff planning and training decisions.

### 📊 Visual Description
An interactive **clustered bar chart** shows, for a selected employee:
- **Red bars**: Average customer satisfaction per job type
- **Blue bars**: Number of days the employee worked on that job type

A slicer lets the user choose any employee by ID.

### 🛠️ Key Implementation Steps
1. **Unpivoted** the `calendar.csv` file in Power Query to convert employee columns into rows.
2. **Merged** the unpivoted calendar data with the job (`landscaping.csv`) and wage (`employees.csv`) data.
3. **Created a unified dataset** named `FactJobs` with one row per employee per job per day.
4. **Built two DAX measures**:
   - `MeanSatisfaction_PerJobType`: Calculates the average satisfaction for a selected employee across all jobs within a job type.
   - `Workdays_PerJobType`: Counts the distinct days an employee worked on each job type.
5. **Formatted job type names** using Power Query (`Text.Proper(Text.Replace(...))`) to make axis labels clean.
6. **Built a clustered bar chart** with shared job type axis:
   - Satisfaction and workdays side-by-side
   - Fully interactive via slicer

![PowerBI_Export_1](https://github.com/user-attachments/assets/6a67c526-1d16-4224-bace-881668cff585)

---

## ✅ Sub-Goal 2: Job Scheduling & Resource Allocation

### 🎯 Objective
Track and visualize how many jobs were **waiting to start** each day. This helps the company detect bottlenecks, seasonal load trends, and resource allocation issues.

A job is considered **waiting** from the **request date until the start date**.

### 📈 Visual Description
A **line chart** shows the number of jobs waiting on each day during the season. A slicer lets the user filter by job type to isolate the impact of specific services.

### 🛠️ Key Implementation Steps
1. In Power Query:
   - Added a custom column to the `landscaping.csv` table that generates a list of **waiting dates** using:
     ```powerquery
     List.Dates([request_date], Duration.Days([start_date] - [request_date]), #duration(1,0,0,0))
     ```
   - Expanded the list to new rows (1 row per day a job was waiting).
   - Renamed this column to `date`.
2. Grouped the data:
   - Grouped by `date` and `job_type`
   - Counted rows to get the number of waiting jobs per day per job type
   - Resulting table: `WaitingJobs`
3. Built a **line chart**:
   - X-axis: `date`
   - Y-axis: `waiting_jobs`
   - Legend: `job_type`
   - Slicer: `job_type` (with "Select All" enabled)

4. Added dynamic titles and tooltips for context.

![PowerBI_Export_2](https://github.com/user-attachments/assets/e4b53375-2a0e-44a4-9f56-4a7485951d79)

---

## 💡 Insights Gained

- Certain employees consistently score higher in satisfaction for specific job types — a potential opportunity to optimize assignments.
- **Basic Lawncare** jobs had the most frequent scheduling delays, especially early in the season.
- By the end of the season, waiting jobs dropped to nearly zero — indicating improvement in backlog management.

---

## 📁 Files

- `landscaping.csv`
- `employees.csv`
- `calendar.csv`
- `Landscaping_Analysis.pbix` — full Power BI report file

---

## 🔐 Notes

- All data is local inside the `.pbix` file.
- No sensitive or private information is used.
- This project was developed entirely in **Power BI Desktop** with no need for Power BI Service or Pro licenses.

---

## 📌 Next Steps

- Expand with employee cost analysis (wages vs. revenue)
- Add a forecast model for future job demand
- Integrate weather data to anticipate seasonal bottlenecks

---



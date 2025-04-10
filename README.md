# Skyline Construction Project Tracker

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

<p align="center">
  <img src="Images/Repository-Structure.png" alt="🗂️ Repository Structure" width="100%">
</p>


```
📁  Skyline Construction Project Tracker 
├── 📁 Dataset  
    └── Project_Management_Dataset.xlsx  
├── 📁 Images  
    └── logo.jpg
    └── money-bag.png
    └── money.png
    └── salary.png
    └── Author.png
    └── DAX Calculations.png
    └── Repository Structure.png
├── 📁 Project Details  
    └── Project Details.pdf  
├── 📁 Project File  
    └── Construction Project Control & Performance Analytics.pbix  
├── 📄 README.md  
├── 📄 LICENSE  
```

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

<p align="center">
  <img src="Images/DAX-Calculations.png" alt="🧮 DAX Calculations" width="100%">
</p>

## 📅 Custom Table: 

#### 1. Calendar

```
Calendar = ADDCOLUMNS(CALENDARAUTO(), "Year", YEAR([Date]), "Month", FORMAT([Date], "mmm"), "Month Number", MONTH([Date]))
```
**📌 Description:** \
This DAX formula creates a dynamic calendar table using CALENDARAUTO(), which automatically detects the date range from the data model. It enriches the table with additional columns:
- Year – Extracts the year from each date.
- Month – Extracts the short month name (e.g., Jan, Feb).
- Month Number – Provides the month as a number (1 to 12).
The calendar table is essential for implementing time intelligence functions such as filtering, grouping, and time-based calculations (YTD, MTD, QTD, etc.).

#### 2. Task Measures

```
Task Measures = DATATABLE("Name", STRING, {{"Measure Placeholder"}})
```
**📌 Description:** \
This DAX formula creates a placeholder table named Task Measures, which acts as a container for storing all task-related DAX measures.
- DATATABLE creates a static table with a single column Name and one dummy row "Measure Placeholder".
- The placeholder row can be deleted later; the table's sole purpose is to group related measures like % Completed, % In Progress, % Not Started, etc.
- Helps maintain clean data modeling and improves readability by organizing all task KPIs in one dedicated table.
- Useful for dashboards that focus on project tracking, task lifecycle, and performance metrics.

#### 3. Budget Measures

```
Budget Measures = DATATABLE("Name", STRING, {{"Measure Placeholder"}})
```
**📌 Description:** \
This DAX formula sets up a dedicated table called Budget Measures, which is used solely to house budget-related DAX measures.
- Like the Task Measures table, it uses a dummy row with DATATABLE and column "Name", which can be removed once actual measures are created.
- It becomes a centralized location for all KPIs related to budget analysis such as:
  - Total Budget
  - Amount Spent
  - Balance
  - Budget Utilization %
  - % Remaining Budget
- Enables clean model organization and intuitive grouping for use in finance dashboards, budget vs actual visuals, and resource planning reports.

## 📐 Calculated Column:

#### 1. Expected Days

```
Expected Days = DATEDIFF('Dataset'[Start Date], 'Dataset'[Due Date], DAY)
```
**📌 Description:** \
This column calculates the number of days between the Start Date and the Due Date. It helps in understanding the expected duration for tasks, tracking deadlines, and managing service-level agreements (SLAs).

## 📏 Measures:

### A. Task Measures

#### 1. Total Tasks

```
Total Tasks = COUNTROWS(FILTER('Dataset', NOT ISBLANK('Dataset'[Task ID])))
```
**📌 Description:** \
Counts only those tasks where the Task ID is not blank. This avoids inflating the task count due to incomplete or missing entries and provides a more accurate KPI for actual tasks in progress or completed.

#### 2. % Completed

```
% Completed = DIVIDE(CALCULATE([Total Tasks], 'Dataset'[Comments] = "No Issues"), [Total Tasks])
```
**📌 Description:** \
Calculates the percentage of tasks with "No Issues" in the Comments column — representing completed or issue-free tasks.
- Uses CALCULATE to filter only such tasks.
- Uses DIVIDE to safely handle division by zero.

#### 3. R % Completed

```
R % Completed = 1.0 - [% Completed]
```
**📌 Description:** \
Represents the remaining (or incomplete) portion of tasks — a simple complement to % Completed. Useful for:
- Highlighting pending workload
- Creating progress vs. backlog visuals
- Adding context to completion KPIs

#### 4. % Not_Started

```
% Not_Started = DIVIDE(CALCULATE([Total Tasks], 'Dataset'[Comments] = "not Started"), [Total Tasks])
```
**📌 Description:** \
Calculates the percentage of tasks where Comments = "not Started", giving visibility into how much of the project is yet to begin.
- Helps identify bottlenecks or areas lacking kickoff
- Useful for resource planning and task delegation

#### 5. R % Not_Started

```
R % Not_Started = 1.0 - [% Not_Started]
```
**📌 Description:** \
Calculates the complement of not started tasks, i.e., tasks that are either in progress or completed. This is especially helpful for:
- Visualizing progress vs. pending side-by-side
- Creating dual indicators (e.g., pie chart: Started vs. Not Started)

#### 6. % Progress

```
% Progress = DIVIDE(CALCULATE([Total Tasks], 'Dataset'[Comments] = "In Progress"), [Total Tasks])
```
**📌 Description:** \
Calculates the percentage of tasks currently in progress based on the Comments column.
- Gives a real-time view of work actively being done
- Complements % Completed and % Not_Started for full project visibility
- Enables dynamic progress tracking across teams or milestones

#### 7. R % Progress

```
R % Progress = 1.0 - [% Progress]
```
**📌 Description:** \
Calculates the remaining portion of tasks that are not in progress, offering a complementary view to % Progress.
- Useful for identifying idle or completed work
- Can be used in dual-indicator visuals (like progress vs. pending)
- Helpful for conditional formatting to highlight workflow imbalances

#### 8. Total Completed Tasks

```
Total Completed Tasks = CALCULATE([Total Tasks], 'Dataset'[Progress %] = 1.0)
```
**📌 Description:** \
Returns the total number of tasks where Progress % is exactly 100% (i.e., fully completed).
- Great for accurate KPI cards
- Enables precise comparison between completed vs. pending tasks
- Useful in time tracking, burndown charts, or milestone analysis

### B. Budget Measures

#### 1. Total Budget

```
Total Budget = SUM('Dataset'[Budget Amount ($)])
```
**📌 Description:**  
Calculates the **sum of all budgeted amounts** across the dataset. It assumes each row contains a valid numeric value in the `'Budget Amount ($)'` column.
- Provides a **cumulative view** of the allocated budget.
- Useful for **high-level budget allocation** cards or visuals.
- Essential base measure for further comparisons like utilization or remaining balance.

#### 2. **Amount Spent**

```
Amount Spent = SUM('Dataset'[Amount Spent ($)])
```
**📌 Description:** \
Sums the actual **expenditure or cost incurred** from the `'Amount Spent ($)'` column.
- Shows how much of the total budget has been utilized.
- Useful for **burn-down charts**, **actuals tracking**, and performance-to-budget metrics.

#### 3. **Balance**

```
Balance = [Total Budget] - [Amount Spent]
```
**📌 Description:** \ 
Calculates the **remaining budget balance** by subtracting the actual amount spent from the total allocated budget.
- Indicates how much budget is **left to spend**.
- Can be used in **forecasting visuals**, budget control indicators, or financial overviews.

#### 4. **Budget Utilization %**

```
Budget Utilization % = DIVIDE([Amount Spent], [Total Budget], 0)
```
**📌 Description:** \ 
Measures the **percentage of the total budget that has been used**.
- Uses `DIVIDE()` to handle division-by-zero gracefully (returns 0 if Total Budget is 0).
- Helps track **budget efficiency** and determine if spending is within acceptable thresholds.

#### 5. **CF Budget** *(Control Flag for Budget)*

```
CF Budget = IF([Budget Utilization %] > 0.5, 0, 1)
```
**📌 Description:** \  
Creates a **control flag** based on budget utilization:
- Returns `1` if utilization is **50% or less** (indicating good control).
- Returns `0` if more than 50% of the budget is already used.
- Useful for **conditional formatting**, visual alerts, or filtering **under/over-budget** projects.

#### 6. **% Remaining Budget**

```
% Remaining Budget = 1.0 - [Budget Utilization %]
```
**📌 Description:** \
Calculates the **remaining portion of the budget** as a percentage.
- Complements `Budget Utilization %` for dual-indicator visuals (e.g., donut chart: Spent vs. Remaining).
- Helps highlight projects with limited funds remaining.

#### 7. **CF Remaining Budget** *(Control Flag based on Remaining %)*

```
CF Remaining Budget = IF([% Remaining Budget] < 0, 0, IF([% Remaining Budget] < 0.3, 1, 2))
```
**📌 Description:** \
Returns a **categorical flag** based on the amount of budget left:
- `0`: Over budget
- `1`: Less than 30% budget remaining (critical)
- `2`: More than 30% remaining (safe zone)
Perfect for use in **traffic light visuals**, risk assessments, or **budget health indicators**.

#### 8. **CF Project Name** *(Flag for Project Status)*

```
CF Project Name = IF([Balance] > 0, 1, 0)
```
**📌 Description:** \
Returns a binary flag to indicate whether a project is still **within budget**.
- `1`: Positive balance (project has remaining budget)
- `0`: Negative or zero balance (at or over budget)
Useful for **filtering visuals**, highlighting compliant vs. non-compliant projects.

#### 9. **Title**

```
Title = IF([% Remaining Budget] < 0, "Above Budget", "Remaining Budget")
```
**📌 Description:** \  
Generates a **dynamic text label** based on the budget status:
- Returns `"Above Budget"` if remaining percentage is negative.
- Returns `"Remaining Budget"` otherwise.
Great for **dynamic titles** or **KPI cards** that reflect real-time budget condition.

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

<p align="center">
  <img src="Images/Author.png" alt="🙋‍♂️ Author" width="100%">
</p>

**Shantanu Prashant Umrani**  
*M.Tech in Data Science and Analytics*  
📧 [umranishantanu@gmail.com](mailto:umranishantanu@gmail.com)  
🔗 [LinkedIn](https://www.linkedin.com/in/shantanu-umrani) | [GitHub](https://github.com/shantanu1109)

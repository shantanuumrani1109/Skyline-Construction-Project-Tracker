# Skyline Construction Project Tracker

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

## 🗂️ Project Structure 

```
📁  Skyline Construction Project Tracker 
├── 📁 Dataset  
    └── Project_Management_Dataset.xlsx  
├── 📁 Images  
    └── logo.jpg  
├── 📁 Project Details  
    └── Project Details.pdf  
├── 📁 Project File  
    └── Project.pbix  
├── 📄 README.md  
├── 📄 LICENSE  
```

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

## 🧮 DAX Calculations

### 📅 Custom Table: 

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

#### 2. KPI Measures

```
KPI Measures = DATATABLE("Name", STRING, {{"Measure Placeholder"}})
```

### 📐 Calculated Column:

#### 1. Expected Days

```
Expected Days = DATEDIFF('Dataset'[Start Date], 'Dataset'[Due Date], DAY)
```
**📌 Description:** \
This column calculates the number of days between the Start Date and the Due Date. It helps in understanding the expected duration for tasks, tracking deadlines, and managing service-level agreements (SLAs).

### 📏 Measures:

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

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

## 🙋‍♂️ Author

**Shantanu Prashant Umrani**  
*M.Tech in Data Science and Analytics*  
📧 [umranishantanu@gmail.com](mailto:umranishantanu@gmail.com)  
🔗 [LinkedIn](https://www.linkedin.com/in/shantanu-umrani) | [GitHub](https://github.com/shantanu1109)

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

<a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"><img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"></a>

## 🙋‍♂️ Author

**Shantanu Prashant Umrani**  
*M.Tech in Data Science and Analytics*  
📧 [umranishantanu@gmail.com](mailto:umranishantanu@gmail.com)  
🔗 [LinkedIn](https://www.linkedin.com/in/shantanu-umrani) | [GitHub](https://github.com/shantanu1109)

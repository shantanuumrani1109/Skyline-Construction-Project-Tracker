# Skyline Construction Project Tracker

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

## 🧮 DAX Calculations

### ✅ 1. Calculated Column: `Expected Days`

```
Expected Days = DATEDIFF('Dataset'[Start Date], 'Dataset'[Due Date], DAY)
```
**Description:** \
This column calculates the number of days between the Start Date and the Due Date. It helps in understanding the expected duration for tasks, tracking deadlines, and managing service-level agreements (SLAs).

## 🙋‍♂️ Author

**Shantanu Prashant Umrani**  
*M.Tech in Data Science and Analytics*  
📧 [umranishantanu@gmail.com](mailto:umranishantanu@gmail.com)  
🔗 [LinkedIn](https://www.linkedin.com/in/shantanu-umrani) | [GitHub](https://github.com/shantanu1109)

# AT1-03 Guide
This guide should assist in the completion of AT1-03.
This task will require:
1. The three CSV files described in the document.
2. [Azure data explorer](https://dataexplorer.azure.com/).

## Learning Objectives
1. Filter data using where
2. Aggregate data using summarise
3. Create new columns using extend
4. Sort results using order by
5. Use top to find top performers
6. Visualise data using render

## KQL Queries
You will now learn more about KQL syntax using the commands from the learning objectives. Examples are provided to help you understand how to write queries for Azure Data Explorer and similar platforms. Try running and modifying these queries to get comfortable with KQL’s structure and capabilities.

1. Filter Enrolments by Grade 
   ```KQL
    Enrolments
    | where Grade > 90
    | join kind=inner (Students) on StudentID
    | project Name, Grade
   ```
   > This query filters the `Enrolments` table to find all enrolments with a grade greater than 90, then joins it with the `Students` table to get the names of those students. The `project` keyword is used to display only the student's name and their grade.

2. Average Grade per Course: 
    ```KQL
    Enrolments
    | summarize AvgGrade = avg(Grade) by CourseID
    | join kind=inner (Courses) on CourseID
    | project CourseName, AvgGrade
    | order by AvgGrade desc
    ```
    > This query calculates the average grade for each course by summarising the `Enrolments` table, grouping by `CourseID`. It then joins with the `Courses` table to get the course names and orders the results by average grade in descending order.

3. Top 5 Students by Average Grade:
    ```KQL
    Enrolments
    | summarize AvgGrade = avg(Grade) by StudentID
    | top 5 by AvgGrade desc
    | join kind=inner (Students) on StudentID
    | project Name, AvgGrade
    ```
    > This query calculates the average grade for each student, selects the top 5 students with the highest average grades, and joins with the `Students` table to display their names along with their average grades.

4. Add a Grade Category:
    ```KQL
    Enrolments
    | extend GradeCategory = case(
    Grade >= 85, "High Distinction",
    Grade >= 75, "Distinction",
    Grade >= 65, "Credit",
    Grade >= 50, "Pass",
    "Fail"
    )
    | join kind=inner (Students) on StudentID
    | project Name, Grade, GradeCategory
    ```
    > This query uses the `extend` operator to create a new column called `GradeCategory` based on the `Grade` values. It categorises grades into "High Distinction", "Distinction", "Credit", "Pass", or "Fail". The results are then joined with the `Students` table to display student names along with their grades and categories.

5. Number of Students per Course:
    ```KQL
    Enrolments
    | summarize EnrolmentCount = count() by CourseID
    | join kind=inner (Courses) on CourseID
    | project CourseName, EnrolmentCount
    | order by EnrolmentCount desc
    ```
    > This query counts the number of students enrolled in each course by summarising the `Enrolments` table, grouping by `CourseID`. It then joins with the `Courses` table to get the course names and orders the results by enrolment count in descending order.

6. Visualise Average Grade per Course:
    ```KQL
    Enrolments
    | summarize AvgGrade = avg(Grade) by CourseID
    | join kind=inner (Courses) on CourseID
    | project CourseName, AvgGrade
    | order by AvgGrade desc
    | render barchart
    ```
    > This query calculates the average grade for each course, joins with the `Courses` table to get course names, and orders the results by average grade in descending order. The `render barchart` command visualises the average grades as a bar chart.


## KQL Challenges
Here are some progressively challenging KQL exercises based on the database tables. These build on the examples above and are designed to deepen your understanding of relational queries.

| Level | Challenge | Tip |
|-------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| 🟢 Beginner | List students who scored above 90 in any course. | Use where and join to filter and combine data.
| 🟢 Beginner | Display all students enrolled in the course "Trip Studies". | Use join and where to filter by course name.
| 🟢 Beginner | Show students enrolled in "Drug Studies", sorted by grade descending.  | Use order by to sort results.
| 🟡 Intermediate | Find the average grade for each course. | Use summarize and join for aggregation.
| 🟡 Intermediate | Add a column that categorizes grades (e.g., A, B, C, D, E and F). | Use extend and case() for conditional logic.
| 🟡 Intermediate | List the top 5 most enrolled courses. | Use summarize, count(), and top.
| 🔴 Advanced | Show average grade per instructor. | Use multi-table joins and aggregation.
| 🔴 Advanced | Show average grade per instructor as a barchart. | Use multi-table joins and aggregation with a barchart.

## KQL Cheat Sheet
This cheat sheet provides a quick reference for common KQL operations and syntax. It can be used as a guide while writing queries in Azure Data Explorer or similar platforms.
| Operation | Syntax | Description |
|-------------|------------------------------------------------------------------------------------------------|
---------------------------------------------------------------------------------------------------|
| Filter | `TableName \| where <condition>` | Filters rows based on a condition. |
| Aggregate | `TableName \| summarize <aggregation> by <column>` | Aggregates data, e.g., `count()`, `avg()`, `sum()`, etc. |
| Join | `TableName \| join kind=<type> (<table>) on <column>` | Combines rows from two tables based on a common column. Types include `inner`, `leftouter`, `rightouter`, `fullouter`, and `anti`. |
| Project | `TableName \| project <column1>, <column2>` | Selects specific columns to display in the results. |
| Extend | `TableName \| extend <new_column> = <expression>` | Adds a new column to the results based on an expression. |
| Order By | `TableName \| order by <column> [asc\|desc]` | Sorts the results by a specified column in ascending or descending order. |
| Top | `TableName \| top <number> by <column> [asc\|desc]` | Selects the top N rows based on a specified column, sorted in ascending or descending order. |
| Render | `TableName \| render <visualization_type>` | Visualises the results in a specified format, such as `barchart`, `piechart`, etc. |
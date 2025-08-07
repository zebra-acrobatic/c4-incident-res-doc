# AT1-03 Guide
This guide should assist in the completion of AT1-03.
This task will require:
1. The three CSV files described in the document.
2. [Azure data explorer](https://dataexplorer.azure.com/).

## KQL Queries
You can use the KQL examples below to quickly learn the basics of Kusto Query Language (KQL) syntax. Each example shows how to select, filter, join, and summarise data, helping you understand how to write queries for Azure Data Explorer and similar platforms. Try running and modifying these queries to get comfortable with KQL’s structure and capabilities.

1. View All students in the table `Students` from the database: 
   ```KQL
   Students
   ```
   > Simply listing the name of a table in a database with KQL will return the entire tables contents.

2. Filter all enrolments for only those who enrolled in 'Company Studies': 
    ```KQL
    Enrolments | where CourseID == 2
    ```
    > In KQL, the `where` keyword is used to filter data in a table by specifying a condition. Only the rows that meet the condition will be included in the results. You can use where with different operators, such as `==`, `!=`, `>`, `<`, or `contains`, to match the data you are interested in.

3. Show student names and emails:
    ```KQL
    Students | project Name, Email
    ```
    > The `project` keyword in KQL is used to select specific columns from a table, allowing you to display only the fields you need in your query results.

4. List student names and their courses:
    ```KQL
    Students | join kind=inner (Enrolments) on StudentID
    ```
    > The `join` keyword in KQL is used to combine rows from two tables based on a related column, allowing you to correlate and analyse data across multiple tables.
    >> **Join kind options in KQL:**  
    >> - `inner`: Returns only matching rows from both tables.  
    >> - `leftouter`: Returns all rows from the left table and matching rows from the right table.  
    >> - `rightouter`: Returns all rows from the right table and matching rows from the left table.  
    >> - `fullouter`: Returns all rows from both tables, with nulls where there is no match.  
    >> - `anti`: Returns rows from the left table that have no matching rows in the right table.

5. Show all the student's full details, the courses they are enrolled in and the full information of each course:
    ```KQL
    Students | join kind=inner (Enrolments) on StudentID | join kind=inner (Courses) on CourseID 
    ```

6. This time, only display the student's names and courses:
    ```KQL
    Students | join kind=inner (Enrolments) on StudentID | join kind=inner (Courses) on CourseID | project Name, CourseName 
    ```
    > The `join` keyword combines data from multiple tables based on a common column, and the `project` keyword selects only the relevant columns, allowing you to display just the information you need from the joined results.

7. Show all the students not enrolled:
    ```KQL 
    Students | join kind=anti (Enrolments) on StudentID
    ```
    > `join kind=anti` returns rows from the left table (Students) that do not have matching rows in the right table (Enrollments) based on StudentID. 

8. Count students per course:
    ```KQL
    Enrolments | summarize StudentCount = count() by CourseID
    ```
    > This query uses the `summarize` operator to count the number of students enrolled in each course, grouping the results by `CourseID` and displaying the total as `StudentCount`.

9. Generate an average grade per course:
    ```KQL
    Enrolments | summarize AvgGrade = avg(Grade) by CourseID
    ```
    > This query calculates the average grade for each course by grouping the Enrolments table by CourseID and applying the avg() aggregation function to the Grade column.

10. Find the top 5 performing students
    ```KQL
    Enrolments | summarize AvgGrade = avg(Grade) by StudentID | top 5 by AvgGrade desc
    ```
    > This query first calculates the average grade for each student using `summarize`, then selects the top 5 students with the highest average grades by sorting in descending order with `top 5 by AvgGrade desc`.

11. Display all students with their average grades:
    ```KQL
    Students | join kind=inner (Enrolments) on StudentID | summarize AvgGrade = avg(Grade) by StudentID | project StudentID, AvgGrade
    ```
    > This query joins the Students table with the Enrolments table, calculates the average grade for each student using `summarize`, and then projects the StudentID and their corresponding average grade

## KQL Challenges
Here are some progressively challenging KQL exercises based on the database tables. These build on the examples above and are designed to deepen your understanding of relational queries.

| Level | Challenge | Tip |
|-------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| 🟢 Beginner | List all course names and their instructors. | Use `project`.
| 🟢 Beginner | Find all students with a grade above 90 in any course. | Use `where`, `join` and `project`.
| 🟢 Beginner | Count how many courses each student is enrolled in  | Use `summarize`, `join` and `project`.
| 🟡 Intermediate | Show the average grade for each student across all their courses. | Use the `summarize` operator to calculate averages and join by `StudentID` to get each student's average grade.
| 🟡 Intermediate | List students who are enrolled in more than 3 courses. | Use `summarize` to count enrolments per student and filter with `where` for those with more than 3 then use `join` to get their details.
| 🟡 Intermediate | Find the top 5 instructors whose courses have the highest average grades. | Join the `Courses` and `Enrolments` tables, then use `summarize` to calculate average grades per instructor and sort the results.
| 🔴 Advanced | Find courses with more than 10 students and an average grade above 80. Display the course name, instructor name, student count, and average grade. | Use `summarize` to count students and calculate average grades, then filter with `where` for courses meeting both conditions. After that, join with the `Courses` and `project` the required columns.
| 🔴 Advanced | Show students who have never scored below 80 in any course. Display only their names and average grades. | Use `summarize` to create a a minimum grade then use `where` to find grades lower than 80. After that combine `Students` and `Enrolments` and display only the desired information.

## KQL Cheat Sheet
- **Basic Syntax**: 
  - `TableName` to select all data from a table.
  - `|` to pipe results from one operation to the next.
- **Filtering**:
  - `| where ColumnName == 'Value'` to filter rows based on a condition.
- **Selecting Columns**:
  - `| project Column1, Column2` to select specific columns.
- **Joining Tables**:
  - `| join kind=inner (OtherTable) on CommonColumn` to combine data from two tables based on a common column.
- **Aggregating Data**:
  - `| summarize NewColumn = aggregationFunction(ColumnName) by GroupingColumn` to perform calculations like count, avg, sum, etc., grouped by a specific column.
- **Sorting**:
  - `| order by ColumnName asc/desc` to sort results in ascending or descending order.
- **Limiting Results**:
  - `| take N` to limit the number of rows returned to N.
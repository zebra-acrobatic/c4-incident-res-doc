# KQL Commands Revision Guide
**Essential Commands for Data Analysis: summarise, extend, order, top, and render**

---

## Overview
Kusto Query Language (KQL) is a powerful tool for analysing large datasets. This guide covers five fundamental commands that every beginner should master: `summarise`, `extend`, `order`, `top`, and `render` (specifically for bar charts).

---

## 1. The `summarise` Command

The `summarise` command groups data and performs aggregate calculations. It's like creating a summary table in Excel.

### Basic Syntax
```kql
TableName
| summarise AggregateFunction(Column) by GroupingColumn
```

### Common Aggregate Functions
- `count()` - counts rows
- `sum()` - adds up values
- `avg()` - calculates average
- `max()` / `min()` - finds highest/lowest values

### Examples

**Example 1: Counting website visits by country**
```kql
WebTraffic
| summarise VisitCount = count() by Country
```
*Result: Shows how many visits came from each country*

**Example 2: Average response time by server**
```kql
ServerLogs
| summarise AvgResponseTime = avg(ResponseTimeMs) by ServerName
```
*Result: Shows the average response time for each server*

**Example 3: Total sales by product category**
```kql
SalesData
| summarise TotalRevenue = sum(SaleAmount) by ProductCategory
```
*Result: Shows total revenue for each product category*

---

## 2. The `extend` Command

The `extend` command adds new calculated columns to your data without changing existing columns. Think of it as adding a formula column in a spreadsheet.

### Basic Syntax
```kql
TableName
| extend NewColumnName = Calculation
```

### Examples

**Example 1: Converting temperature from Celsius to Fahrenheit**
```kql
WeatherData
| extend TempFahrenheit = (TempCelsius * 9/5) + 32
```

**Example 2: Calculating profit margin**
```kql
ProductSales
| extend ProfitMargin = ((SalePrice - Cost) / SalePrice) * 100
```

**Example 3: Extracting day of week from datetime**
```kql
EventLogs
| extend DayOfWeek = dayofweek(EventDateTime)
```

**Example 4: Creating status categories**
```kql
ServerMetrics
| extend StatusCategory = case(
    CPUUsage > 90, "Critical",
    CPUUsage > 70, "High",
    CPUUsage > 50, "Medium",
    "Normal"
)
```

---

## 3. The `order` Command

The `order` command sorts your results. Use `asc` for ascending (lowest to highest) or `desc` for descending (highest to lowest).

### Basic Syntax
```kql
TableName
| order by ColumnName asc/desc
```

### Examples

**Example 1: Ordering by timestamp (most recent first)**
```kql
ApplicationLogs
| order by Timestamp desc
```

**Example 2: Ordering by multiple columns**
```kql
StudentGrades
| order by Subject asc, Grade desc
```
*Result: Sorts by subject alphabetically, then by grade (highest first) within each subject*

**Example 3: Ordering calculated values**
```kql
EmployeeData
| extend FullName = strcat(FirstName, " ", LastName)
| order by FullName asc
```

---

## 4. The `top` Command

The `top` command returns the highest or lowest values from a dataset. It's particularly useful for finding "top 10" lists.

### Basic Syntax
```kql
TableName
| top NumberOfRows by ColumnName asc/desc
```

### Examples

**Example 1: Top 5 cities by population**
```kql
CityData
| top 5 by Population desc
```

**Example 2: Bottom 3 performing stores by sales**
```kql
StoreSales
| top 3 by TotalSales asc
```

**Example 3: Top 10 error types by frequency**
```kql
ErrorLogs
| summarise ErrorCount = count() by ErrorType
| top 10 by ErrorCount desc
```

---

## 5. The `render` Command for Bar Charts

The `render` command creates visualisations from your data. We'll focus on bar charts, which are excellent for comparing categories.

### Basic Syntax
```kql
TableName
| render barchart
```

### Bar Chart Examples

**Example 1: Simple bar chart of sales by region**
```kql
RegionalSales
| summarise TotalSales = sum(SalesAmount) by Region
| render barchart
```

**Example 2: Top 10 products by sales volume**
```kql
ProductSales
| summarise UnitsSold = sum(Quantity) by ProductName
| top 10 by UnitsSold desc
| render barchart
```

**Example 3: Monthly website traffic**
```kql
WebsiteMetrics
| extend Month = format_datetime(Date, "yyyy-MM")
| summarise PageViews = sum(Views) by Month
| order by Month asc
| render barchart
```

**Example 4: Error distribution by severity level**
```kql
SystemErrors
| summarise ErrorCount = count() by SeverityLevel
| order by ErrorCount desc
| render barchart
```

---

## Combining Commands: Real-World Examples

### Example 1: Top 5 Australian cities by average temperature
```kql
WeatherReadings
| where Country == "Australia"
| extend TempFahrenheit = (TempCelsius * 9/5) + 32
| summarise AvgTemp = avg(TempFahrenheit) by City
| top 5 by AvgTemp desc
| render barchart
```

### Example 2: Monthly revenue trend with profit margins
```kql
MonthlySales
| extend Month = format_datetime(SaleDate, "yyyy-MM")
| extend ProfitMargin = ((Revenue - Costs) / Revenue) * 100
| summarise TotalRevenue = sum(Revenue), AvgMargin = avg(ProfitMargin) by Month
| order by Month asc
| render barchart
```

### Example 3: Server performance analysis
```kql
ServerPerformance
| extend PerformanceCategory = case(
    ResponseTime > 1000, "Slow",
    ResponseTime > 500, "Average", 
    "Fast"
)
| summarise ServerCount = count() by PerformanceCategory
| order by ServerCount desc
| render barchart
```

---

## Key Tips for Success

1. **Always test with small datasets first** - Use `| take 100` to limit results whilst learning
2. **Use meaningful column names** - `summarise VisitorCount = count()` is better than `summarise count()`
3. **Order your operations logically** - Filter first, then extend, summarise, order, and finally render
4. **Remember Australian date formats** - KQL typically uses ISO format (YYYY-MM-DD)
5. **Bar charts work best with categorical data** - Use them to compare different groups or categories

---

## Common Patterns to Remember

```kql
// Pattern 1: Analysis with visualisation
DataTable
| summarise Metric = aggregation(Column) by Category
| top N by Metric desc
| render barchart

// Pattern 2: Time-based analysis
DataTable
| extend TimeGroup = format_datetime(DateColumn, "format")
| summarise Aggregation = function(Value) by TimeGroup
| order by TimeGroup asc
| render barchart

// Pattern 3: Calculated fields with grouping
DataTable
| extend NewField = calculation
| summarise Summary = function(NewField) by GroupBy
| order by Summary desc
| render barchart
```

Practice these patterns with different datasets to build your confidence with KQL!
# KQL (Kusto Query Language) Revision Notes

## What is KQL?

KQL (Kusto Query Language) is a powerful query language designed for analysing large datasets in real-time. Originally developed by Microsoft for Azure services, it's widely used across Australia in government agencies, banks, telecommunications companies, and tech organisations for log analysis, security monitoring, and business intelligence.

## What is KQL Used For?

- **Security Analysis**: Detecting cyber threats and suspicious activities
- **System Monitoring**: Tracking application performance and server health
- **Business Intelligence**: Analysing customer behaviour and sales data
- **Compliance Reporting**: Meeting regulatory requirements (APRA, ACMA, etc.)
- **Troubleshooting**: Identifying system issues and outages

Common Australian use cases include monitoring banking transactions for fraud detection, analysing telecommunications network performance, and tracking government service usage patterns.

## Basic KQL Structure

KQL queries follow a simple pattern:
```
DataTable
| operator1
| operator2
| operator3
```

Each line represents a step in processing your data, similar to a pipeline.

## Essential KQL Commands

### 1. **where** - Filtering Data

The `where` command filters rows based on specified conditions.

**Syntax**: `| where condition`

**Examples**:
```kql
// Find all login events from Sydney users
SecurityEvent
| where City == "Sydney"
| where EventType == "Login"

// Find high-value transactions in Australian dollars
TransactionLogs
| where Amount > 10000 and Currency == "AUD"

// Find errors in the last 24 hours
ApplicationLogs
| where TimeGenerated > ago(1d)
| where Level == "Error"
```

**Common operators**:
- `==` (equals), `!=` (not equals)
- `>`, `<`, `>=`, `<=` (comparisons)
- `and`, `or` (logical operators)
- `contains`, `startswith`, `endswith` (string matching)

### 2. **project** - Selecting Columns

The `project` command selects which columns to display or creates new calculated columns.

**Syntax**: `| project column1, column2, newColumn = expression`

**Examples**:
```kql
// Show only relevant columns for Melbourne office security events
SecurityEvent
| where Office == "Melbourne"
| project TimeGenerated, UserName, EventType, IPAddress

// Create calculated fields for GST analysis
SalesData
| project ProductName, BasePrice, GST = BasePrice * 0.1, TotalPrice = BasePrice * 1.1

// Rename columns for clearer reporting
WebLogs
| project Timestamp = TimeGenerated, User = UserPrincipalName, Page = RequestURL
```

### 3. **summarize** - Aggregating Data

The `summarize` command groups data and performs calculations like counting, summing, or averaging.

**Syntax**: `| summarize aggregation by grouping_column`

**Examples**:
```kql
// Count login attempts by Australian state
LoginEvents
| summarize LoginCount = count() by State

// Average transaction value by bank branch
BankTransactions
| summarize AvgTransaction = avg(Amount), TotalTransactions = count() by BranchCode

// Daily error counts for troubleshooting
ApplicationLogs
| where Level == "Error"
| summarize ErrorCount = count() by bin(TimeGenerated, 1d)

// Top 5 most accessed government services
GovernmentPortalLogs
| summarize AccessCount = count() by ServiceName
| top 5 by AccessCount
```

**Common aggregation functions**:
- `count()` - count rows
- `sum()` - add values
- `avg()` - calculate average
- `max()`, `min()` - find highest/lowest values
- `dcount()` - count distinct values

### 4. **join** - Combining Tables

The `join` command combines data from two tables based on matching columns.

**Syntax**: `Table1 | join Table2 on ColumnName`

**Examples**:
```kql
// Match user activities with their department information
UserActivity
| join (EmployeeDirectory | project UserID, Department, Location) on UserID
| where Location == "Perth"

// Combine transaction data with merchant details
TransactionData
| join kind=inner (MerchantDirectory | project MerchantID, BusinessName, ABN) on MerchantID
| where BusinessName contains "Woolworths"

// Link security alerts with user profiles
SecurityAlerts
| join (UserProfiles | project UserID, RiskLevel, Manager) on UserID
| where RiskLevel == "High"
```

**Join types**:
- `inner` (default) - only matching records from both tables
- `left` - all records from left table, matching from right
- `right` - all records from right table, matching from left

### 5. **sort/order by** - Ordering Results

Sort results in ascending or descending order.

**Examples**:
```kql
// Latest transactions first
BankTransactions
| sort by TimeGenerated desc

// Highest value transactions for audit
TransactionLogs
| where Currency == "AUD"
| sort by Amount desc
| take 100
```

### 6. **take/limit** - Limiting Results

Restrict the number of rows returned.

**Examples**:
```kql
// Show first 50 login attempts today
LoginEvents
| where TimeGenerated > startofday(now())
| take 50

// Top 10 error messages by frequency
ErrorLogs
| summarize Count = count() by ErrorMessage
| sort by Count desc
| limit 10
```

## Practical Australian Examples

### Example 1: Banking Fraud Detection
```kql
// Identify suspicious after-hours transactions in major Australian cities
BankTransactions
| where TimeGenerated between (datetime(2024-01-15 18:00) .. datetime(2024-01-16 06:00))
| where Amount > 5000 and Currency == "AUD"
| where City in ("Sydney", "Melbourne", "Brisbane", "Perth")
| summarize SuspiciousCount = count(), TotalAmount = sum(Amount) by AccountNumber, City
| where SuspiciousCount > 3
| sort by TotalAmount desc
```

### Example 2: Government Service Performance
```kql
// Analyse Medicare online service usage across states
GovernmentServices
| where ServiceName == "Medicare Online"
| where TimeGenerated > ago(7d)
| summarize 
    TotalSessions = count(),
    UniqueUsers = dcount(UserID),
    AvgSessionDuration = avg(SessionDuration)
    by State
| sort by TotalSessions desc
```

### Example 3: Telecommunications Network Monitoring
```kql
// Find network outages affecting regional Australia
NetworkEvents
| where EventType == "Outage"
| where Region startswith "Regional"
| join (TowerDirectory | project TowerID, Location, Postcode) on TowerID
| summarize 
    OutageCount = count(),
    AffectedCustomers = sum(CustomerCount)
    by Location, Postcode
| where OutageCount > 2
| sort by AffectedCustomers desc
```

## Key Tips for Beginners

1. **Start Simple**: Begin with basic filtering using `where`, then add complexity
2. **Use Comments**: Add `//` to document your queries for future reference
3. **Test Incrementally**: Build queries step by step, checking results at each stage
4. **Mind the Data Types**: Ensure you're comparing like with like (strings with strings, numbers with numbers)
5. **Time Ranges**: Always consider time ranges to avoid querying excessive data
6. **Case Sensitivity**: KQL is case-sensitive for operators and functions

## Common Australian Data Scenarios

- **Financial Services**: Transaction monitoring, compliance reporting, fraud detection
- **Telecommunications**: Network performance, customer usage patterns, fault diagnosis
- **Government**: Service delivery metrics, security monitoring, citizen engagement analysis
- **Healthcare**: Patient flow analysis, resource utilisation, compliance tracking
- **Retail**: Sales analysis, inventory management, customer behaviour tracking

Remember: KQL is designed to handle massive datasets efficiently. Start with these basic commands, practice with realistic scenarios, and gradually build more complex queries as your confidence grows.
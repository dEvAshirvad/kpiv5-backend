# Statistics API Documentation

**Base URL:** `http://localhost:3001/api/v1/entries/statistics`

## Table of Contents

1. [Authentication](#authentication)
2. [Statistics Endpoint](#statistics-endpoint)
3. [Response Format](#response-format)
4. [Filtering Examples](#filtering-examples)
5. [Data Models](#data-models)

---

## Authentication

All endpoints require authentication. Include the appropriate authorization header:

```
Authorization: Bearer <your-jwt-token>
```

---

## Statistics Endpoint

### Get Ranking and Statistics Data

**GET** `/entries/statistics`

#### Query Parameters

| Parameter    | Type   | Required | Description              |
| ------------ | ------ | -------- | ------------------------ |
| `department` | string | No       | Filter by department     |
| `role`       | string | No       | Filter by role           |
| `month`      | number | No       | Filter by month (1-12)   |
| `year`       | number | No       | Filter by year (>= 2000) |

#### Example Requests

**Get all statistics:**

```
GET /entries/statistics
```

**Filter by department:**

```
GET /entries/statistics?department=it
```

**Filter by role:**

```
GET /entries/statistics?role=manager
```

**Filter by month and year:**

```
GET /entries/statistics?month=8&year=2025
```

**Combined filters:**

```
GET /entries/statistics?department=it&role=manager&month=8&year=2025
```

---

## Response Format

### Success Response (200)

```json
{
  "success": true,
  "message": "Statistics fetched successfully",
  "filters": {
    "department": "it",
    "role": "manager",
    "month": 8,
    "year": 2025
  },
  "statistics": {
    "totalEntries": 25,
    "averageScore": 78.5,
    "maxScore": 95,
    "minScore": 45,
    "departmentStats": [
      {
        "department": "it",
        "totalEntries": 15,
        "averageScore": 82.3,
        "topScore": 95
      },
      {
        "department": "hr",
        "totalEntries": 10,
        "averageScore": 72.8,
        "topScore": 88
      }
    ],
    "roleStats": [
      {
        "role": "manager",
        "totalEntries": 12,
        "averageScore": 85.2,
        "topScore": 95
      },
      {
        "role": "developer",
        "totalEntries": 13,
        "averageScore": 72.1,
        "topScore": 88
      }
    ]
  },
  "availableFilters": {
    "departments": ["it", "hr", "finance", "marketing"],
    "roles": ["manager", "developer", "analyst", "coordinator"],
    "months": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    "years": [2024, 2025]
  },
  "ranking": [
    {
      "rank": 1,
      "entryId": "68a35af672170dd3a5f31f9e",
      "employee": {
        "_id": "68a2eca1072d46dfb2287dbb",
        "name": "John Doe",
        "contact": {
          "email": "john.doe@example.com",
          "phone": "+1234567890"
        },
        "department": "it",
        "departmentRole": "manager"
      },
      "template": {
        "_id": "68a3004c5da043dcc8722a3c",
        "name": "Performance Evaluation Template",
        "description": "Template for evaluating employee performance",
        "role": "manager",
        "frequency": "daily",
        "departmentSlug": "it"
      },
      "month": 8,
      "year": 2025,
      "score": 95,
      "status": "generated",
      "kpiNames": [
        {
          "label": "Task Completion Rate",
          "value": "High Priority"
        }
      ],
      "values": [
        {
          "key": "taskcompletionrate",
          "value": 95,
          "score": 95,
          "subKpis": [
            {
              "key": "darj",
              "value": 100
            },
            {
              "key": "nirakrit",
              "value": 95
            }
          ]
        }
      ],
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    },
    {
      "rank": 2,
      "entryId": "68a35af672170dd3a5f31f9f",
      "employee": {
        "_id": "68a2eca1072d46dfb2287dbc",
        "name": "Jane Smith",
        "contact": {
          "email": "jane.smith@example.com",
          "phone": "+1234567891"
        },
        "department": "it",
        "departmentRole": "developer"
      },
      "template": {
        "_id": "68a3004c5da043dcc8722a3d",
        "name": "Code Quality Template",
        "description": "Template for evaluating code quality",
        "role": "developer",
        "frequency": "weekly",
        "departmentSlug": "it"
      },
      "month": 8,
      "year": 2025,
      "score": 88,
      "status": "generated",
      "kpiNames": [
        {
          "label": "Code Quality Score",
          "value": "Critical"
        }
      ],
      "values": [
        {
          "key": "codequalityscore",
          "value": 88,
          "score": 88,
          "subKpis": []
        }
      ],
      "createdAt": "2024-01-15T11:30:00.000Z",
      "updatedAt": "2024-01-15T11:30:00.000Z"
    }
  ]
}
```

---

## Filtering Examples

### 1. All Departments Ranking

```bash
GET /entries/statistics
```

Returns ranking across all departments and roles.

### 2. Department-Specific Ranking

```bash
GET /entries/statistics?department=it
```

Returns ranking for IT department only.

### 3. Role-Specific Ranking

```bash
GET /entries/statistics?role=manager
```

Returns ranking for manager role across all departments.

### 4. Time-Based Filtering

```bash
GET /entries/statistics?month=8&year=2025
```

Returns ranking for August 2025.

### 5. Combined Filters

```bash
GET /entries/statistics?department=it&role=manager&month=8&year=2025
```

Returns ranking for IT department managers in August 2025.

---

## Data Models

### Statistics Response

```json
{
  "filters": {
    "department": "string (optional)",
    "role": "string (optional)",
    "month": "number (optional, 1-12)",
    "year": "number (optional, >= 2000)"
  },
  "statistics": {
    "totalEntries": "number",
    "averageScore": "number (rounded to 2 decimal places)",
    "maxScore": "number",
    "minScore": "number",
    "departmentStats": [
      {
        "department": "string",
        "totalEntries": "number",
        "averageScore": "number",
        "topScore": "number"
      }
    ],
    "roleStats": [
      {
        "role": "string",
        "totalEntries": "number",
        "averageScore": "number",
        "topScore": "number"
      }
    ]
  },
  "availableFilters": {
    "departments": ["string array"],
    "roles": ["string array"],
    "months": ["number array"],
    "years": ["number array"]
  },
  "ranking": [
    {
      "rank": "number (1-based)",
      "entryId": "string",
      "employee": {
        "_id": "string",
        "name": "string",
        "contact": {
          "email": "string",
          "phone": "string"
        },
        "department": "string",
        "departmentRole": "string"
      },
      "template": {
        "_id": "string",
        "name": "string",
        "description": "string",
        "role": "string",
        "frequency": "string",
        "departmentSlug": "string"
      },
      "month": "number",
      "year": "number",
      "score": "number",
      "status": "string",
      "kpiNames": ["array"],
      "values": ["array"],
      "createdAt": "string (ISO date)",
      "updatedAt": "string (ISO date)"
    }
  ]
}
```

---

## Key Features

### 1. **Flexible Filtering**

- Filter by department, role, month, year
- Combine multiple filters
- Get available filter options

### 2. **Comprehensive Statistics**

- Overall statistics (total, average, max, min scores)
- Department-wise statistics
- Role-wise statistics

### 3. **Ranking System**

- Sorted by score (highest first)
- Full employee and template data
- Complete entry details

### 4. **Available Filters**

- Dynamic list of available departments
- Dynamic list of available roles
- Available months and years

### 5. **Data Enrichment**

- Employee data (name, contact, department, role)
- Template data (name, description, role, frequency)
- Complete entry data (KPIs, values, scores)

---

## Error Responses

### Invalid Filter (400)

```json
{
  "success": false,
  "message": "Invalid filter parameter",
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### No Data Found (200)

```json
{
  "success": true,
  "message": "Statistics fetched successfully",
  "filters": {},
  "statistics": {
    "totalEntries": 0,
    "averageScore": 0,
    "maxScore": 0,
    "minScore": 0,
    "departmentStats": [],
    "roleStats": []
  },
  "availableFilters": {
    "departments": [],
    "roles": [],
    "months": [],
    "years": []
  },
  "ranking": []
}
```

---

## Notes

- **Ranking**: Entries are sorted by score in descending order (highest score first)
- **Statistics**: Calculated based on filtered data
- **Available Filters**: Dynamically generated from existing data
- **Employee Data**: Full employee information included (not populated, merged)
- **Template Data**: Full template information included (not populated, merged)
- **Score Calculation**: Based on KPI values and template maxMarks
- **Filtering**: Applied in memory after fetching all entries for accurate statistics

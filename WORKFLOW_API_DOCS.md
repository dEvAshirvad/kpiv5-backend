# KPI Entry Workflow API Documentation

**Base URL:** `http://localhost:3001/api/v1`

## Table of Contents

1. [Authentication](#authentication)
2. [Workflow Overview](#workflow-overview)
3. [Employee Workflow Endpoints](#employee-workflow-endpoints)
4. [Template Workflow Endpoints](#template-workflow-endpoints)
5. [Entry Workflow Endpoints](#entry-workflow-endpoints)
6. [Complete Workflow Example](#complete-workflow-example)

---

## Authentication

All endpoints require authentication. Include the appropriate authorization header:

```
Authorization: Bearer <your-jwt-token>
```

---

## Workflow Overview

The KPI Entry workflow follows these steps:

1. **Choose Employee** - Select employee by department or role
2. **Choose Template** - Get templates matching employee's department and role
3. **Choose Month/Year** - Select period for KPI entry
4. **Get/Create Entry** - Retrieve existing entry or create new one
5. **Generate Form** - Get form structure from template
6. **Submit Entry** - Create or update KPI entry

---

## Employee Workflow Endpoints

### 1. Get Employees by Department

**GET** `/employees/department/:department`

#### Path Parameters

| Parameter    | Type   | Required | Description     |
| ------------ | ------ | -------- | --------------- |
| `department` | string | Yes      | Department name |

#### Example Request

```
GET /employees/department/it
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Department employees fetched successfully",
  "employees": [
    {
      "_id": "65a1b2c3d4e5f6789012346",
      "name": "John Doe",
      "contact": {
        "email": "john.doe@example.com",
        "phone": "+1234567890"
      },
      "department": "it",
      "departmentRole": "manager",
      "metadata": {},
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    },
    {
      "_id": "65a1b2c3d4e5f6789012347",
      "name": "Jane Smith",
      "contact": {
        "email": "jane.smith@example.com",
        "phone": "+1234567891"
      },
      "department": "it",
      "departmentRole": "developer",
      "metadata": {},
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 2. Get Employees by Department Role

**GET** `/employees/role/:departmentRole`

#### Path Parameters

| Parameter        | Type   | Required | Description     |
| ---------------- | ------ | -------- | --------------- |
| `departmentRole` | string | Yes      | Department role |

#### Example Request

```
GET /employees/role/manager
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Department role employees fetched successfully",
  "employees": [
    {
      "_id": "65a1b2c3d4e5f6789012346",
      "name": "John Doe",
      "contact": {
        "email": "john.doe@example.com",
        "phone": "+1234567890"
      },
      "department": "it",
      "departmentRole": "manager",
      "metadata": {},
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

## Template Workflow Endpoints

### 1. Get Templates by Employee Department and Role

**GET** `/templates/employee/:employeeId`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `employeeId` | string | Yes      | Employee ID |

#### Example Request

```
GET /templates/employee/65a1b2c3d4e5f6789012346
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Employee department templates fetched successfully",
  "templates": [
    {
      "_id": "65a1b2c3d4e5f6789012347",
      "name": "Performance Evaluation Template",
      "description": "Template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [
        {
          "name": "Task Completion Rate",
          "description": "Percentage of tasks completed on time",
          "maxMarks": 100,
          "kpiType": "percentage",
          "metric": "(completed_tasks / total_tasks) * 100",
          "kpiUnit": "%",
          "isDynamic": true,
          "subKpis": [
            {
              "name": "दर्ज",
              "key": "darj",
              "value_type": "number"
            },
            {
              "name": "निराकृत",
              "key": "nirakrit",
              "value_type": "number"
            }
          ]
        }
      ],
      "createdBy": "user123",
      "updatedBy": "user123",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 2. Generate Form Structure from Template

**GET** `/templates/:templateId/form-structure`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `templateId` | string | Yes      | Template ID |

#### Example Request

```
GET /templates/65a1b2c3d4e5f6789012347/form-structure
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Form structure generated successfully",
  "formStructure": {
    "templateId": "65a1b2c3d4e5f6789012347",
    "templateName": "Performance Evaluation Template",
    "templateDescription": "Template for evaluating employee performance",
    "frequency": "daily",
    "role": "manager",
    "departmentSlug": "it",
    "kpis": [
      {
        "name": "Task Completion Rate",
        "description": "Percentage of tasks completed on time",
        "maxMarks": 100,
        "kpiType": "percentage",
        "metric": "(completed_tasks / total_tasks) * 100",
        "kpiUnit": "%",
        "isDynamic": true,
        "key": "taskcompletionrate",
        "subKpis": [
          {
            "name": "दर्ज",
            "key": "darj",
            "value_type": "number"
          },
          {
            "name": "निराकृत",
            "key": "nirakrit",
            "value_type": "number"
          }
        ]
      },
      {
        "name": "Code Quality Score",
        "description": "Code quality assessment score",
        "maxMarks": 100,
        "kpiType": "percentage",
        "metric": "quality_score",
        "kpiUnit": "%",
        "isDynamic": false,
        "key": "codequalityscore",
        "subKpis": [
          {
            "name": "दर्ज",
            "key": "darj",
            "value_type": "number"
          },
          {
            "name": "निराकृत",
            "key": "nirakrit",
            "value_type": "number"
          }
        ]
      }
    ]
  }
}
```

---

## Entry Workflow Endpoints

### 1. Get or Create Entry for Workflow

**GET** `/entries/workflow/:employeeId/:templateId/:month/:year`

#### Path Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| `employeeId` | string | Yes      | Employee ID    |
| `templateId` | string | Yes      | Template ID    |
| `month`      | number | Yes      | Month (1-12)   |
| `year`       | number | Yes      | Year (>= 2000) |

#### Example Request

```
GET /entries/workflow/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347/7/2025
```

#### Success Response (200) - Existing Entry

```json
{
  "success": true,
  "message": "Existing entry found",
  "entry": {
    "_id": "65a1b2c3d4e5f6789012345",
    "employeeId": {
      "_id": "65a1b2c3d4e5f6789012346",
      "name": "John Doe",
      "contact": {
        "email": "john.doe@example.com",
        "phone": "+1234567890"
      },
      "department": "IT"
    },
    "templateId": {
      "_id": "65a1b2c3d4e5f6789012347",
      "name": "Performance Evaluation Template",
      "description": "Template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily"
    },
    "month": 7,
    "year": 2025,
    "kpiNames": [
      {
        "label": "Task Completion Rate",
        "value": "High Priority"
      }
    ],
    "values": [
      {
        "key": "taskcompletionrate",
        "value": 85,
        "score": 85,
        "subKpis": [
          {
            "key": "darj",
            "value": 100
          },
          {
            "key": "nirakrit",
            "value": 85
          }
        ]
      }
    ],
    "score": 85,
    "status": "initiated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  },
  "isNew": false
}
```

#### Success Response (200) - New Entry Created

```json
{
  "success": true,
  "message": "New entry created",
  "entry": {
    "_id": "65a1b2c3d4e5f6789012345",
    "employeeId": {
      "_id": "65a1b2c3d4e5f6789012346",
      "name": "John Doe",
      "contact": {
        "email": "john.doe@example.com",
        "phone": "+1234567890"
      },
      "department": "IT"
    },
    "templateId": {
      "_id": "65a1b2c3d4e5f6789012347",
      "name": "Performance Evaluation Template",
      "description": "Template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily"
    },
    "month": 7,
    "year": 2025,
    "kpiNames": [
      {
        "label": "Task Completion Rate",
        "value": ""
      },
      {
        "label": "Code Quality Score",
        "value": ""
      }
    ],
    "values": [
      {
        "key": "taskcompletionrate",
        "value": 0,
        "score": 0,
        "subKpis": [
          {
            "key": "darj",
            "value": 0
          },
          {
            "key": "nirakrit",
            "value": 0
          }
        ]
      },
      {
        "key": "codequalityscore",
        "value": 0,
        "score": 0,
        "subKpis": [
          {
            "key": "darj",
            "value": 0
          },
          {
            "key": "nirakrit",
            "value": 0
          }
        ]
      }
    ],
    "score": 0,
    "status": "initiated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  },
  "isNew": true
}
```

---

### 2. Get Available Months and Years

**GET** `/entries/available/:employeeId/:templateId`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `employeeId` | string | Yes      | Employee ID |
| `templateId` | string | Yes      | Template ID |

#### Example Request

```
GET /entries/available/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Available months and years fetched successfully",
  "entries": [
    {
      "_id": "65a1b2c3d4e5f6789012345",
      "month": 7,
      "year": 2025,
      "status": "initiated"
    },
    {
      "_id": "65a1b2c3d4e5f6789012346",
      "month": 6,
      "year": 2025,
      "status": "generated"
    },
    {
      "_id": "65a1b2c3d4e5f6789012347",
      "month": 5,
      "year": 2025,
      "status": "generated"
    }
  ]
}
```

---

### 3. Get Entry Summary for Employee

**GET** `/entries/summary/:employeeId`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `employeeId` | string | Yes      | Employee ID |

#### Example Request

```
GET /entries/summary/65a1b2c3d4e5f6789012346
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry summary fetched successfully",
  "summary": {
    "Performance Evaluation Template": {
      "2025-07": {
        "entryId": "65a1b2c3d4e5f6789012345",
        "status": "initiated",
        "score": 177,
        "month": 7,
        "year": 2025
      },
      "2025-06": {
        "entryId": "65a1b2c3d4e5f6789012346",
        "status": "generated",
        "score": 185,
        "month": 6,
        "year": 2025
      },
      "2025-05": {
        "entryId": "65a1b2c3d4e5f6789012347",
        "status": "generated",
        "score": 192,
        "month": 5,
        "year": 2025
      }
    },
    "Code Quality Template": {
      "2025-07": {
        "entryId": "65a1b2c3d4e5f6789012348",
        "status": "inprogress",
        "score": 85,
        "month": 7,
        "year": 2025
      }
    }
  }
}
```

---

## Complete Workflow Example

Here's a complete example of the KPI entry workflow:

### Step 1: Choose Employee

```bash
# Get employees by department
GET /employees/department/it

# Response: List of IT department employees
```

### Step 2: Choose Template

```bash
# Get templates for selected employee
GET /templates/employee/65a1b2c3d4e5f6789012346

# Response: Templates matching employee's department and role
```

### Step 3: Choose Month/Year

```bash
# Get available months/years for employee and template
GET /entries/available/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347

# Response: Available periods with status
```

### Step 4-5: Get or Create Entry

```bash
# Get or create entry for specific period
GET /entries/workflow/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347/7/2025

# Response: Existing entry or newly created entry with template structure
```

### Step 6: Generate Form Structure

```bash
# Get form structure from template
GET /templates/65a1b2c3d4e5f6789012347/form-structure

# Response: Complete form structure with KPIs and validation rules
```

### Step 7: Submit Entry

```bash
# For new entry
POST /entries
{
  "employeeId": "65a1b2c3d4e5f6789012346",
  "templateId": "65a1b2c3d4e5f6789012347",
  "month": 7,
  "year": 2025,
  "kpiNames": [...],
  "values": [...],
  "status": "initiated"
}

# For existing entry
PUT /entries/65a1b2c3d4e5f6789012345
{
  "values": [...],
  "status": "inprogress"
}
```

---

## Key Features

1. **Smart Employee Filtering**: Filter employees by department or role
2. **Template Matching**: Automatically find templates matching employee's department and role
3. **Period Management**: Track available months/years for entries
4. **Automatic Entry Creation**: Create entries with proper structure if not exists
5. **Form Generation**: Generate complete form structure from template
6. **Entry Summary**: Overview of all entries for an employee
7. **Automatic Score Calculation**: Scores calculated based on template maxMarks

---

## Error Handling

All endpoints return consistent error responses:

```json
{
  "success": false,
  "message": "Error description",
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

Common error scenarios:

- Employee not found
- Template not found
- Invalid month/year
- Duplicate entry creation
- Validation errors

---

## Notes

- All endpoints require authentication
- Employee and template data is auto-populated in responses
- Scores are automatically calculated based on template maxMarks
- Form structure includes all validation rules from template
- Workflow supports both new entry creation and existing entry updates

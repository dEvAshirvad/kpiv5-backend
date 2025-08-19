# Entry API Documentation

**Base URL:** `http://localhost:3001/api/v1/entries`

## Table of Contents

1. [Authentication](#authentication)
2. [Error Responses](#error-responses)
3. [Entry Endpoints](#entry-endpoints)
4. [Status Management](#status-management)
5. [Filter Endpoints](#filter-endpoints)
6. [Utility Endpoints](#utility-endpoints)
7. [Workflow Endpoints](#workflow-endpoints)

---

## Authentication

All endpoints require authentication. Include the appropriate authorization header:

```
Authorization: Bearer <your-jwt-token>
```

---

## Error Responses

### Standard Error Format

```json
{
  "success": false,
  "message": "Error description",
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### Common HTTP Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `404` - Not Found
- `500` - Internal Server Error

---

## Entry Endpoints

### 1. Get All Entries

**GET** `/entries`

#### Query Parameters

| Parameter    | Type   | Required | Description                                         |
| ------------ | ------ | -------- | --------------------------------------------------- |
| `page`       | number | No       | Page number (default: 1)                            |
| `limit`      | number | No       | Items per page (default: 10)                        |
| `search`     | string | No       | Search in KPI names                                 |
| `employeeId` | string | No       | Filter by employee ID                               |
| `templateId` | string | No       | Filter by template ID                               |
| `month`      | number | No       | Filter by month (1-12)                              |
| `year`       | number | No       | Filter by year                                      |
| `status`     | string | No       | Filter by status (initiated, inprogress, generated) |

#### Example Request

```
GET /entries?page=1&limit=5&search=performance&employeeId=65a1b2c3d4e5f6789012345&month=7&year=2025&status=initiated
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entries fetched successfully",
  "docs": [
    {
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
        },
        {
          "label": "Code Quality Score",
          "value": "Critical"
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
        },
        {
          "key": "codequalityscore",
          "value": 92,
          "score": 92,
          "subKpis": [
            {
              "key": "darj",
              "value": 95
            },
            {
              "key": "nirakrit",
              "value": 92
            }
          ]
        }
      ],
      "score": 177,
      "status": "initiated",
      "dataSource": "manual",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ],
  "total": 25,
  "page": 1,
  "limit": 5,
  "totalPages": 5,
  "hasNextPage": true,
  "hasPreviousPage": false
}
```

---

### 2. Get Entry by ID

**GET** `/entries/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Entry ID    |

#### Example Request

```
GET /entries/65a1b2c3d4e5f6789012345
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry fetched successfully",
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
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Entry not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 3. Create Entry

**POST** `/entries`

#### Request Body

```json
{
  "employeeId": "65a1b2c3d4e5f6789012346",
  "templateId": "65a1b2c3d4e5f6789012347",
  "month": 7,
  "year": 2025,
  "kpiNames": [
    {
      "label": "Task Completion Rate",
      "value": "High Priority"
    },
    {
      "label": "Code Quality Score",
      "value": "Critical"
    }
  ],
  "values": [
    {
      "key": "taskcompletionrate",
      "value": 85,
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
    },
    {
      "key": "codequalityscore",
      "value": 92,
      "subKpis": [
        {
          "key": "darj",
          "value": 95
        },
        {
          "key": "nirakrit",
          "value": 92
        }
      ]
    }
  ],
  "status": "initiated",
  "dataSource": "manual"
}
```

#### Success Response (201)

```json
{
  "success": true,
  "message": "Entry created successfully",
  "entry": {
    "_id": "65a1b2c3d4e5f6789012345",
    "employeeId": "65a1b2c3d4e5f6789012346",
    "templateId": "65a1b2c3d4e5f6789012347",
    "month": 7,
    "year": 2025,
    "kpiNames": [
      {
        "label": "Task Completion Rate",
        "value": "High Priority"
      },
      {
        "label": "Code Quality Score",
        "value": "Critical"
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
      },
      {
        "key": "codequalityscore",
        "value": 92,
        "score": 92,
        "subKpis": [
          {
            "key": "darj",
            "value": 95
          },
          {
            "key": "nirakrit",
            "value": 92
          }
        ]
      }
    ],
    "score": 177,
    "status": "initiated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

#### Validation Error Response (400)

```json
{
  "success": false,
  "message": "Template not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

```json
{
  "success": false,
  "message": "Missing KPIs: Code Quality Score",
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

```json
{
  "success": false,
  "message": "Missing sub-KPIs for Task Completion Rate: darj, nirakrit",
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 4. Update Entry

**PUT** `/entries/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Entry ID    |

#### Request Body (Partial Update)

```json
{
  "values": [
    {
      "key": "taskcompletionrate",
      "value": 90,
      "subKpis": [
        {
          "key": "darj",
          "value": 100
        },
        {
          "key": "nirakrit",
          "value": 90
        }
      ]
    }
  ],
  "status": "inprogress"
}
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry updated successfully",
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
        "value": 90,
        "score": 90,
        "subKpis": [
          {
            "key": "darj",
            "value": 100
          },
          {
            "key": "nirakrit",
            "value": 90
          }
        ]
      }
    ],
    "score": 90,
    "status": "inprogress",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T11:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Entry not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 5. Delete Entry

**DELETE** `/entries/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Entry ID    |

#### Example Request

```
DELETE /entries/65a1b2c3d4e5f6789012345
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry deleted successfully",
  "entry": {
    "_id": "65a1b2c3d4e5f6789012345",
    "employeeId": "65a1b2c3d4e5f6789012346",
    "templateId": "65a1b2c3d4e5f6789012347",
    "month": 7,
    "year": 2025,
    "kpiNames": [...],
    "values": [...],
    "score": 177,
    "status": "initiated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Entry not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

## Status Management

### Update Entry Status

**PUT** `/entries/:id/status`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Entry ID    |

#### Request Body

```json
{
  "status": "generated"
}
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry status updated successfully",
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
    "kpiNames": [...],
    "values": [...],
    "score": 177,
    "status": "generated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T12:30:00.000Z"
  }
}
```

---

## Filter Endpoints

### 1. Get Entries by Employee

**GET** `/entries/employee/:employeeId`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `employeeId` | string | Yes      | Employee ID |

#### Example Request

```
GET /entries/employee/65a1b2c3d4e5f6789012346
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Employee entries fetched successfully",
  "entries": [
    {
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
      "kpiNames": [...],
      "values": [...],
      "score": 177,
      "status": "initiated",
      "dataSource": "manual",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 2. Get Entries by Template

**GET** `/entries/template/:templateId`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `templateId` | string | Yes      | Template ID |

#### Example Request

```
GET /entries/template/65a1b2c3d4e5f6789012347
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template entries fetched successfully",
  "entries": [
    {
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
      "kpiNames": [...],
      "values": [...],
      "score": 177,
      "status": "initiated",
      "dataSource": "manual",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 3. Get Entries by Month and Year

**GET** `/entries/month/:month/year/:year`

#### Path Parameters

| Parameter | Type   | Required | Description    |
| --------- | ------ | -------- | -------------- |
| `month`   | number | Yes      | Month (1-12)   |
| `year`    | number | Yes      | Year (>= 2000) |

#### Example Request

```
GET /entries/month/7/year/2025
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Month-year entries fetched successfully",
  "entries": [
    {
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
      "kpiNames": [...],
      "values": [...],
      "score": 177,
      "status": "initiated",
      "dataSource": "manual",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 4. Get Entries by Status

**GET** `/entries/status/:status`

#### Path Parameters

| Parameter | Type   | Required | Description                               |
| --------- | ------ | -------- | ----------------------------------------- |
| `status`  | string | Yes      | Status (initiated, inprogress, generated) |

#### Example Request

```
GET /entries/status/initiated
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Status entries fetched successfully",
  "entries": [
    {
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
      "kpiNames": [...],
      "values": [...],
      "score": 177,
      "status": "initiated",
      "dataSource": "manual",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

## Utility Endpoints

### 1. Check Entry Exists

**GET** `/entries/check/:employeeId/:templateId/:month/:year`

#### Path Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| `employeeId` | string | Yes      | Employee ID    |
| `templateId` | string | Yes      | Template ID    |
| `month`      | number | Yes      | Month (1-12)   |
| `year`       | number | Yes      | Year (>= 2000) |

#### Example Request

```
GET /entries/check/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347/7/2025
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry existence checked successfully",
  "exists": true
}
```

---

### 2. Get Entry by Employee, Template, Month, Year

**GET** `/entries/find/:employeeId/:templateId/:month/:year`

#### Path Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| `employeeId` | string | Yes      | Employee ID    |
| `templateId` | string | Yes      | Template ID    |
| `month`      | number | Yes      | Month (1-12)   |
| `year`       | number | Yes      | Year (>= 2000) |

#### Example Request

```
GET /entries/find/65a1b2c3d4e5f6789012346/65a1b2c3d4e5f6789012347/7/2025
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Entry fetched successfully",
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
    "kpiNames": [...],
    "values": [...],
    "score": 177,
    "status": "initiated",
    "dataSource": "manual",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Entry not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

## Workflow Endpoints

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
    "kpiNames": [...],
    "values": [...],
    "score": 177,
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

## Data Models

### Entry Schema

```json
{
  "employeeId": "string (required)",
  "templateId": "string (required)",
  "month": "number (required, 1-12)",
  "year": "number (required, >= 2000)",
  "kpiNames": [
    {
      "label": "string (required)",
      "value": "string (optional)"
    }
  ],
  "values": [
    {
      "key": "string (required)",
      "value": "number (optional, 0-100)",
      "score": "number (auto-calculated, >= 0)",
      "subKpis": [
        {
          "key": "string (required)",
          "value": "number (optional, >= 0)"
        }
      ]
    }
  ],
  "score": "number (auto-calculated, >= 0)",
  "status": "enum: initiated, inprogress, generated (required)",
  "dataSource": "string (optional)"
}
```

---

## Validation Rules

1. **Template Validation:**
   - KPI names must match template
   - Values structure must match template
   - Sub-KPIs must include 'darj' and 'nirakrit'

2. **Data Validation:**
   - Month: 1-12
   - Year: >= 2000
   - Values: 0-100 (percentage)
   - Scores: >= 0
   - Sub-KPI values: >= 0

3. **Uniqueness:**
   - No duplicate entries for same employee, template, month, year

4. **Status Flow:**
   - `initiated` → `inprogress` → `generated`
   - Status can be updated via dedicated endpoint

---

## Status Definitions

- **initiated**: Entry initiated for that month till its generated
- **inprogress**: Updates to values till its generated
- **generated**: End of template frequency or by nodal officer

---

## Rate Limiting

- 100 requests per minute per IP address
- 1000 requests per hour per user

---

## Notes

- All timestamps are in ISO 8601 format
- IDs are MongoDB ObjectIds
- Search is case-insensitive
- Pagination starts from page 1
- Employee and template data is auto-populated in responses
- Template validation ensures data integrity
- Unique constraint prevents duplicate entries for same employee, template, month, year
- **Score Calculation**: Individual KPI scores are automatically calculated as `(value / 100) * maxMarks` from the template
- **Total Score**: Sum of all individual KPI scores
- **Score Updates**: Scores are automatically recalculated when values are updated
- **Workflow Support**: Complete workflow endpoints for step-by-step KPI entry process

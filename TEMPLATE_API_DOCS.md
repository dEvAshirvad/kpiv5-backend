# Template API Documentation

**Base URL:** `http://localhost:3001/templates`

## Table of Contents

1. [Authentication](#authentication)
2. [Error Responses](#error-responses)
3. [Template Endpoints](#template-endpoints)
4. [Template Version Endpoints](#template-version-endpoints)
5. [Filter Endpoints](#filter-endpoints)

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

## Template Endpoints

### 1. Get All Templates

**GET** `/templates`

#### Query Parameters

| Parameter        | Type   | Required | Description                                             |
| ---------------- | ------ | -------- | ------------------------------------------------------- |
| `page`           | number | No       | Page number (default: 1)                                |
| `limit`          | number | No       | Items per page (default: 10)                            |
| `search`         | string | No       | Search in name, description, role                       |
| `departmentSlug` | string | No       | Filter by department slug                               |
| `frequency`      | string | No       | Filter by frequency (daily, weekly, monthly, quarterly) |
| `role`           | string | No       | Filter by role                                          |

#### Example Request

```
GET /templates?page=1&limit=5&search=performance&departmentSlug=it&frequency=daily
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Templates fetched successfully",
  "docs": [
    {
      "_id": "65a1b2c3d4e5f6789012345",
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
        },
        {
          "name": "Code Quality Score",
          "description": "Code quality assessment score",
          "maxMarks": 100,
          "kpiType": "percentage",
          "metric": "quality_score",
          "kpiUnit": "%",
          "isDynamic": false,
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

### 2. Get Template by ID

**GET** `/templates/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Template ID |

#### Example Request

```
GET /templates/65a1b2c3d4e5f6789012345
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template fetched successfully",
  "template": {
    "_id": "65a1b2c3d4e5f6789012345",
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
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Template not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 3. Create Template

**POST** `/templates`

#### Request Body

```json
{
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
    },
    {
      "name": "Code Quality Score",
      "description": "Code quality assessment score",
      "maxMarks": 100,
      "kpiType": "percentage",
      "metric": "quality_score",
      "kpiUnit": "%",
      "isDynamic": false,
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
  "updatedBy": "user123"
}
```

#### Success Response (201)

```json
{
  "success": true,
  "message": "Template created successfully",
  "template": {
    "_id": "65a1b2c3d4e5f6789012345",
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
}
```

#### Validation Error Response (400)

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    {
      "field": "template",
      "message": "At least 2 KPIs required"
    },
    {
      "field": "template.0.subKpis",
      "message": "Sub-KPIs must include 'दर्ज' and 'निराकृत'"
    }
  ],
  "status": 400,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 4. Update Template

**PUT** `/templates/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Template ID |

#### Request Body (Partial Update)

```json
{
  "name": "Updated Performance Template",
  "description": "Updated description for performance evaluation",
  "template": [
    {
      "name": "Updated Task Completion Rate",
      "description": "Updated description",
      "maxMarks": 150,
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
  "updatedBy": "user456"
}
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template updated successfully",
  "template": {
    "_id": "65a1b2c3d4e5f6789012345",
    "name": "Updated Performance Template",
    "description": "Updated description for performance evaluation",
    "role": "manager",
    "frequency": "daily",
    "departmentSlug": "it",
    "template": [
      {
        "name": "Updated Task Completion Rate",
        "description": "Updated description",
        "maxMarks": 150,
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
    "updatedBy": "user456",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T11:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Template not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

### 5. Delete Template

**DELETE** `/templates/:id`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | string | Yes      | Template ID |

#### Example Request

```
DELETE /templates/65a1b2c3d4e5f6789012345
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template deleted successfully",
  "template": {
    "_id": "65a1b2c3d4e5f6789012345",
    "name": "Performance Evaluation Template",
    "description": "Template for evaluating employee performance",
    "role": "manager",
    "frequency": "daily",
    "departmentSlug": "it",
    "template": [...],
    "createdBy": "user123",
    "updatedBy": "user123",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Template not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

## Template Version Endpoints

### 1. Create Template Version

**POST** `/templates/versions`

#### Request Body

```json
{
  "templateId": "65a1b2c3d4e5f6789012345",
  "version": 2,
  "name": "Performance Evaluation Template v2",
  "description": "Updated template for evaluating employee performance",
  "role": "manager",
  "frequency": "daily",
  "departmentSlug": "it",
  "template": [
    {
      "name": "Task Completion Rate v2",
      "description": "Updated percentage of tasks completed on time",
      "maxMarks": 120,
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
  "createdBy": "user456"
}
```

#### Success Response (201)

```json
{
  "success": true,
  "message": "Template version created successfully",
  "version": {
    "_id": "65a1b2c3d4e5f6789012346",
    "templateId": "65a1b2c3d4e5f6789012345",
    "version": 2,
    "name": "Performance Evaluation Template v2",
    "description": "Updated template for evaluating employee performance",
    "role": "manager",
    "frequency": "daily",
    "departmentSlug": "it",
    "template": [...],
    "createdBy": "user456",
    "createdAt": "2024-01-15T12:30:00.000Z",
    "updatedAt": "2024-01-15T12:30:00.000Z"
  }
}
```

---

### 2. Get Template Versions

**GET** `/templates/:templateId/versions`

#### Path Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `templateId` | string | Yes      | Template ID |

#### Example Request

```
GET /templates/65a1b2c3d4e5f6789012345/versions
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template versions fetched successfully",
  "versions": [
    {
      "_id": "65a1b2c3d4e5f6789012346",
      "templateId": "65a1b2c3d4e5f6789012345",
      "version": 2,
      "name": "Performance Evaluation Template v2",
      "description": "Updated template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [...],
      "createdBy": "user456",
      "createdAt": "2024-01-15T12:30:00.000Z",
      "updatedAt": "2024-01-15T12:30:00.000Z"
    },
    {
      "_id": "65a1b2c3d4e5f6789012347",
      "templateId": "65a1b2c3d4e5f6789012345",
      "version": 1,
      "name": "Performance Evaluation Template v1",
      "description": "Original template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [...],
      "createdBy": "user123",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 3. Get Specific Template Version

**GET** `/templates/:templateId/versions/:version`

#### Path Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| `templateId` | string | Yes      | Template ID    |
| `version`    | number | Yes      | Version number |

#### Example Request

```
GET /templates/65a1b2c3d4e5f6789012345/versions/2
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Template version fetched successfully",
  "version": {
    "_id": "65a1b2c3d4e5f6789012346",
    "templateId": "65a1b2c3d4e5f6789012345",
    "version": 2,
    "name": "Performance Evaluation Template v2",
    "description": "Updated template for evaluating employee performance",
    "role": "manager",
    "frequency": "daily",
    "departmentSlug": "it",
    "template": [...],
    "createdBy": "user456",
    "createdAt": "2024-01-15T12:30:00.000Z",
    "updatedAt": "2024-01-15T12:30:00.000Z"
  }
}
```

#### Error Response (404)

```json
{
  "success": false,
  "message": "Template version not found",
  "status": 404,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

---

## Filter Endpoints

### 1. Get Templates by Department

**GET** `/templates/department/:departmentSlug`

#### Path Parameters

| Parameter        | Type   | Required | Description     |
| ---------------- | ------ | -------- | --------------- |
| `departmentSlug` | string | Yes      | Department slug |

#### Example Request

```
GET /templates/department/it
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Department templates fetched successfully",
  "templates": [
    {
      "_id": "65a1b2c3d4e5f6789012345",
      "name": "Performance Evaluation Template",
      "description": "Template for evaluating employee performance",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [...],
      "createdBy": "user123",
      "updatedBy": "user123",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 2. Get Templates by Frequency

**GET** `/templates/frequency/:frequency`

#### Path Parameters

| Parameter   | Type   | Required | Description                                   |
| ----------- | ------ | -------- | --------------------------------------------- |
| `frequency` | string | Yes      | Frequency (daily, weekly, monthly, quarterly) |

#### Example Request

```
GET /templates/frequency/daily
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Frequency templates fetched successfully",
  "templates": [
    {
      "_id": "65a1b2c3d4e5f6789012345",
      "name": "Daily Performance Template",
      "description": "Daily performance evaluation template",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [...],
      "createdBy": "user123",
      "updatedBy": "user123",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

### 3. Get Templates by Role

**GET** `/templates/role/:role`

#### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `role`    | string | Yes      | Role name   |

#### Example Request

```
GET /templates/role/manager
```

#### Success Response (200)

```json
{
  "success": true,
  "message": "Role templates fetched successfully",
  "templates": [
    {
      "_id": "65a1b2c3d4e5f6789012345",
      "name": "Manager Performance Template",
      "description": "Template for manager performance evaluation",
      "role": "manager",
      "frequency": "daily",
      "departmentSlug": "it",
      "template": [...],
      "createdBy": "user123",
      "updatedBy": "user123",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

---

## Data Models

### Template Schema

```json
{
  "name": "string (required)",
  "description": "string (required)",
  "role": "string (required)",
  "frequency": "enum: daily, weekly, monthly, quarterly (required)",
  "departmentSlug": "string (required)",
  "template": [
    {
      "name": "string (required)",
      "description": "string (required)",
      "maxMarks": "number (required, min: 0)",
      "kpiType": "enum: percentage (required)",
      "metric": "string (required)",
      "kpiUnit": "enum: % (required)",
      "isDynamic": "boolean (required)",
      "subKpis": [
        {
          "name": "string (required)",
          "key": "string (required)",
          "value_type": "enum: number (required)"
        }
      ]
    }
  ],
  "createdBy": "string (required)",
  "updatedBy": "string (required)"
}
```

### Template Version Schema

```json
{
  "templateId": "string (required)",
  "version": "number (required)",
  "name": "string (required)",
  "description": "string (required)",
  "role": "string (required)",
  "frequency": "enum: daily, weekly, monthly, quarterly (required)",
  "departmentSlug": "string (required)",
  "template": "array (same as Template)",
  "createdBy": "string (required)"
}
```

---

## Validation Rules

1. **Template Array**: Must contain at least 2 KPIs
2. **KPI Names**: Must be unique within a template
3. **Sub-KPIs**: Must include at least 2 sub-KPIs with keys 'darj' and 'nirakrit'
4. **Frequency**: Must be one of: daily, weekly, monthly, quarterly
5. **KPI Type**: Must be 'percentage'
6. **KPI Unit**: Must be '%'
7. **Max Marks**: Must be non-negative integer

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
- Version numbers must be sequential integers starting from 1

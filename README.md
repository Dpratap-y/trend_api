# Trends API Requirements Document

## Overview

This document outlines the requirements for implementing two new API endpoints for ticket and revenue trends data in the admin donor application.

## API Endpoints Required

### 1. Ticket Trends API

**Endpoint:** `GET /api/v1/trends/tickets`

**Purpose:** Retrieve ticket sales trends with service usage data

**Authentication:** Required (Bearer token)

**Request Payload:**

```json
{
  "startDate": "2024-01-15",
  "endDate": "2024-01-21",
  "period": "daily",
  "channel": null
}
```

**Request Parameters:**
| Parameter | Type | Required | Description | Values |
|-----------|------|----------|-------------|---------|
| `startDate` | string | ✅ | Start date for trend data | Format: "YYYY-MM-DD" |
| `endDate` | string | ✅ | End date for trend data | Format: "YYYY-MM-DD" |
| `period` | string | ✅ | Data aggregation period | "daily", "weekly", "monthly" |
| `channel` | number | ❌ | Filter by channel | `null` (all), `1` (online), `2` (offline) |

**Response Format:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Ticket trends retrieved successfully",
  "data": {
    "dateRangeData": [
      {
        "date": "2024-01-15",
        "tickets": 1250,
        "visitors": 1250,
        "online": 800,
        "offline": 450,
        "adults": 950,
        "children": 300,
        "vipCount": 25
      }
    ],
    "serviceUsage": {
      "kioskStats": {
        "KisokAdult": 1850,
        "KisokChildren": 920,
        "Books": 320,
        "Prasadam": 450,
        "Puja": 380,
        "Temples": 680,
        "HanumanVadamala": 180,
        "GarudaPuja": 140,
        "HanumanPuja": 200,
        "SamathaNeerajanam": 95,
        "AudioDevices": 420,
        "Beverages": 580,
        "DressCode": 220,
        "PrivateGuide": 70,
        "GroupGuide": 280
      },
      "offlineStats": {
        "KisokAdult": 1420,
        "KisokChildren": 680,
        "Books": 250,
        "Prasadam": 350,
        "Puja": 280,
        "Temples": 520,
        "HanumanVadamala": 120,
        "GarudaPuja": 100,
        "HanumanPuja": 150,
        "SamathaNeerajanam": 70,
        "AudioDevices": 320,
        "Beverages": 450,
        "DressCode": 180,
        "PrivateGuide": 50,
        "GroupGuide": 220
      }
    }
  }
}
```

---

### 2. Revenue Trends API

**Endpoint:** `GET /api/v1/trends/revenue`

**Purpose:** Retrieve revenue trends with payment method breakdown

**Authentication:** Required (Bearer token)

**Request Payload:**

```json
{
  "startDate": "2024-01-15",
  "endDate": "2024-01-21",
  "period": "daily",
  "channel": null,
  "paymentMethod": null
}
```

**Request Parameters:**
| Parameter | Type | Required | Description | Values |
|-----------|------|----------|-------------|---------|
| `startDate` | string | ✅ | Start date for trend data | Format: "YYYY-MM-DD" |
| `endDate` | string | ✅ | End date for trend data | Format: "YYYY-MM-DD" |
| `period` | string | ✅ | Data aggregation period | "daily", "weekly", "monthly" |
| `channel` | number | ❌ | Filter by channel | `null` (all), `1` (online), `2` (offline) |
| `paymentMethod` | string | ❌ | Filter by payment method | `null` (all), "UPI", "Card", "Cash" |

**Response Format:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Revenue trends retrieved successfully",
  "data": {
    "dateRangeData": [
      {
        "date": "2024-01-15",
        "amount": 187500,
        "tickets": 1250,
        "visitors": 1250,
        "online": 800,
        "offline": 450,
        "onlineAmount": 120000,
        "offlineAmount": 67500,
        "averageTicketPrice": 150
      }
    ],
    "paymentMethods": [
      {
        "name": "UPI",
        "amount": 450000,
        "count": 3000,
        "percentage": 40.0
      },
      {
        "name": "Card",
        "amount": 337500,
        "count": 2250,
        "percentage": 30.0
      },
      {
        "name": "Cash",
        "amount": 337500,
        "count": 2250,
        "percentage": 30.0
      }
    ]
  }
}
```

---

## Data Aggregation Rules

### Period Formats

- **Daily**: `"2024-01-15"` (YYYY-MM-DD)
- **Weekly**: `"2024-W03"` (YYYY-W##)
- **Monthly**: `"2024-01"` (YYYY-MM)

### Channel Mapping

- `1` = Online transactions
- `2` = Offline transactions
- `null` = All channels combined

### Payment Method Values

- `"UPI"` = UPI payments
- `"Card"` = Card payments (Credit/Debit)
- `"Cash"` = Cash payments
- `null` = All payment methods

---

## Error Responses

### 400 Bad Request

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Invalid request parameters",
  "errors": {
    "startDate": "startDate is required",
    "endDate": "endDate must be after startDate"
  }
}
```

### 401 Unauthorized

```json
{
  "success": false,
  "statusCode": 401,
  "message": "Authentication required"
}
```

### 500 Internal Server Error

```json
{
  "success": false,
  "statusCode": 500,
  "message": "Internal server error"
}
```

---

## Business Logic Requirements

### Data Sources

- **Ticket Data**: From existing ticket reports database
- **Revenue Data**: From payment transaction records
- **Service Usage**: From kiosk and offline service logs

### Performance Considerations

- **Caching**: Implement caching for frequently requested date ranges
- **Pagination**: For large date ranges, consider pagination
- **Rate Limiting**: Apply appropriate rate limits

### Data Validation

- Validate date ranges (startDate < endDate)
- Validate period values
- Validate channel and payment method values
- Ensure user has permission to access the data

---

## Frontend Integration Notes

### Current Usage

These APIs will be consumed by:

- `src/features/ticket-reports/components/trends/ticket-trends-tab.tsx`
- `src/features/ticket-reports/components/trends/revenue-trends-tab.tsx`
- `src/features/ticket-reports/hooks/use-ticket-reports.ts`

### Expected Response Times

- **Daily data (7 days)**: < 500ms
- **Weekly data (4 weeks)**: < 800ms
- **Monthly data (12 months)**: < 1000ms

---

## Testing Requirements

### Test Cases

1. **Valid requests** with all parameter combinations
2. **Invalid date ranges** (startDate > endDate)
3. **Invalid period values**
4. **Invalid channel/payment method values**
5. **Authentication failures**
6. **Large date ranges** (performance testing)
7. **Empty result sets** (no data for date range)

### Sample Test Data

Provide sample data for:

- 7 days of daily data
- 4 weeks of weekly data
- 12 months of monthly data
- All channel combinations
- All payment method combinations

---

## Priority

**High Priority** - Required for ticket reports dashboard functionality

## Timeline

**Target Completion**: [Insert target date]

## Contact

**Frontend Team**: [Your contact information]
**Questions**: Please reach out for any clarifications on requirements

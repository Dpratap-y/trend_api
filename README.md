# Trends API Requirements Document

## Overview

This document outlines the requirements for implementing three new API endpoints for trends data in the admin donor application.

## API Endpoints Required

### 1. Ticket Trends API

**Endpoint:** `GET /api/v1/trends/tickets`

**Purpose:** Retrieve ticket sales trends with daily breakdown

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
        "vipCount": 25,
        "channels": {
          "online": {
            "tickets": 800,
            "visitors": 800,
            "adults": 600,
            "children": 200,
            "vipCount": 15
          },
          "offline": {
            "tickets": 450,
            "visitors": 450,
            "adults": 350,
            "children": 100,
            "vipCount": 10
          }
        }
      },
      {
        "date": "2024-01-16",
        "tickets": 1380,
        "visitors": 1380,
        "online": 920,
        "offline": 460,
        "adults": 1050,
        "children": 330,
        "vipCount": 28,
        "channels": {
          "online": {
            "tickets": 920,
            "visitors": 920,
            "adults": 690,
            "children": 230,
            "vipCount": 18
          },
          "offline": {
            "tickets": 460,
            "visitors": 460,
            "adults": 360,
            "children": 100,
            "vipCount": 10
          }
        }
      }
    ]
  }
}
```

---

### 2. Revenue Trends API

**Endpoint:** `GET /api/v1/trends/revenue`

**Purpose:** Retrieve revenue trends with payment method breakdown per day

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
        "averageTicketPrice": 150,
        "channels": {
          "online": {
            "amount": 120000,
            "tickets": 800,
            "visitors": 800,
            "averageTicketPrice": 150
          },
          "offline": {
            "amount": 67500,
            "tickets": 450,
            "visitors": 450,
            "averageTicketPrice": 150
          }
        },
        "paymentMethods": [
          {
            "name": "UPI",
            "amount": 75000,
            "count": 500,
            "percentage": 40.0,
            "channels": {
              "online": {
                "amount": 48000,
                "count": 320
              },
              "offline": {
                "amount": 27000,
                "count": 180
              }
            }
          },
          {
            "name": "Card",
            "amount": 56250,
            "count": 375,
            "percentage": 30.0,
            "channels": {
              "online": {
                "amount": 36000,
                "count": 240
              },
              "offline": {
                "amount": 20250,
                "count": 135
              }
            }
          },
          {
            "name": "Cash",
            "amount": 56250,
            "count": 375,
            "percentage": 30.0,
            "channels": {
              "online": {
                "amount": 36000,
                "count": 240
              },
              "offline": {
                "amount": 20250,
                "count": 135
              }
            }
          }
        ]
      },
      {
        "date": "2024-01-16",
        "amount": 207000,
        "tickets": 1380,
        "visitors": 1380,
        "online": 920,
        "offline": 460,
        "onlineAmount": 138000,
        "offlineAmount": 69000,
        "averageTicketPrice": 150,
        "channels": {
          "online": {
            "amount": 138000,
            "tickets": 920,
            "visitors": 920,
            "averageTicketPrice": 150
          },
          "offline": {
            "amount": 69000,
            "tickets": 460,
            "visitors": 460,
            "averageTicketPrice": 150
          }
        },
        "paymentMethods": [
          {
            "name": "UPI",
            "amount": 82800,
            "count": 552,
            "percentage": 40.0,
            "channels": {
              "online": {
                "amount": 55200,
                "count": 368
              },
              "offline": {
                "amount": 27600,
                "count": 184
              }
            }
          },
          {
            "name": "Card",
            "amount": 62100,
            "count": 414,
            "percentage": 30.0,
            "channels": {
              "online": {
                "amount": 41400,
                "count": 276
              },
              "offline": {
                "amount": 20700,
                "count": 138
              }
            }
          },
          {
            "name": "Cash",
            "amount": 62100,
            "count": 414,
            "percentage": 30.0,
            "channels": {
              "online": {
                "amount": 41400,
                "count": 276
              },
              "offline": {
                "amount": 20700,
                "count": 138
              }
            }
          }
        ]
      }
    ]
  }
}
```

---

### 3. Service Usage API (NEW)

**Endpoint:** `GET /api/v1/trends/service-usage`

**Purpose:** Retrieve service usage trends (separate from ticket/revenue trends)

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
  "message": "Service usage trends retrieved successfully",
  "data": {
    "dateRangeData": [
      {
        "date": "2024-01-15",
        "channels": {
          "online": {
            "KisokAdult": 400,
            "KisokChildren": 200,
            "Books": 60,
            "Prasadam": 100,
            "Puja": 80,
            "Temples": 150,
            "HanumanVadamala": 40,
            "GarudaPuja": 30,
            "HanumanPuja": 45,
            "SamathaNeerajanam": 20,
            "AudioDevices": 90,
            "Beverages": 120,
            "DressCode": 50,
            "PrivateGuide": 15,
            "GroupGuide": 60
          },
          "offline": {
            "KisokAdult": 300,
            "KisokChildren": 150,
            "Books": 45,
            "Prasadam": 75,
            "Puja": 60,
            "Temples": 120,
            "HanumanVadamala": 30,
            "GarudaPuja": 25,
            "HanumanPuja": 35,
            "SamathaNeerajanam": 15,
            "AudioDevices": 70,
            "Beverages": 90,
            "DressCode": 40,
            "PrivateGuide": 10,
            "GroupGuide": 45
          }
        },
        "total": {
          "KisokAdult": 700,
          "KisokChildren": 350,
          "Books": 105,
          "Prasadam": 175,
          "Puja": 140,
          "Temples": 270,
          "HanumanVadamala": 70,
          "GarudaPuja": 55,
          "HanumanPuja": 80,
          "SamathaNeerajanam": 35,
          "AudioDevices": 160,
          "Beverages": 210,
          "DressCode": 90,
          "PrivateGuide": 25,
          "GroupGuide": 105
        }
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

## Key Changes from Previous Version

1. **Removed service usage** from ticket and revenue trends APIs
2. **Added separate service usage API** for service-specific data
3. **Added channel-wise breakdown** in each day's data
4. **Added payment method breakdown** per day with channel-wise data
5. **Simplified ticket trends** to focus only on ticket/visitor metrics
6. **Enhanced revenue trends** with detailed payment method and channel data

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
- **Service Usage Data**: From kiosk and offline service logs

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
- `src/features/ticket-reports/components/trends/service-usage-chart.tsx`
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

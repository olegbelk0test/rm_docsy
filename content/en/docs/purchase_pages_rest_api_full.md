---
tags: [purchase-page, developers, rest, api]
sidebar_position: 70
sidebar_label: Purchase Pages REST API
sidebar_class_name: green
---

> **Note** Names and internal tool references have been anonymised for portfolio use.

# Purchase Pages REST API

This reference describes the HTTPS endpoint used to retrieve purchase‑page configurations.

---

## 1. Endpoint Overview

| Method | Path | Purpose |
|--------|------|---------|
| **GET** | `/v1/purchase-pages` | Retrieve one or more **PurchasePage** objects filtered by `pageTypeId`. |

### Required Headers

| Header | Example | Comment |
|--------|---------|---------|
| `Authorization` | `Bearer eyJhbGciOiJI…` | OAuth 2 / JWT token |
| `Accept` | `application/json` | — |
| `X-Request-Id` (optional) | `550e8400-e29b-41d4-a716-446655440000` | Correlates logs & enables idempotency |

---

## 2. GET `/v1/purchase-pages`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `pageTypeId` | int or repeated int | **Yes** | Repeat to pass multiple values, e.g. `pageTypeId=1&pageTypeId=5` |
| `locale` | string | No | IETF tag (`en-US`, `fr-FR`). Defaults to player locale |

### Sample Request

```http
GET /v1/purchase-pages?pageTypeId=1&pageTypeId=42 HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIs…
Accept: application/json
```

### Successful Response — `200 OK`

```json
[
  {
    "pageId": 9123,
    "pageTypeId": 1,
    "startDate": "2025-06-15T00:00:00Z",
    "endDate":   "2025-06-30T23:59:59Z",
    "resolveRequestId": "38dc0f13-3949-4bf8-9df1-48c40a743d85",
    "options": [
      { "productId": "coins_pack_small", "price": 2.99, "currency": "USD", "bonusPercent": 0 },
      { "productId": "coins_pack_large", "price": 9.99, "currency": "USD", "bonusPercent": 20 }
    ],
    "additionalData": {
      "backgroundArtUrl": "https://cdn.example.com/img/promo/bg_9123.png",
      "badgeText": "20% BONUS"
    }
  }
]
```

### Error Responses

| HTTP status | `error.code` | Maps to (SDK) | Typical action |
|-------------|--------------|---------------|----------------|
| 401 Unauthorized | `IDENTITY_ERROR` | `IdentityException` | Refresh token & retry |
| 503 Service Unavailable | `NO_INTERNET` | `NoInternet` | Retry with back‑off |
| 500 Internal Server Error | `GENERIC_ERROR` | `GenericException` | Log & show generic error |

**Error envelope**

```json
{
  "error": { "code": "IDENTITY_ERROR", "message": "Access token expired." },
  "requestId": "550e8400-e29b-41d4-a716-446655440000"
}
```

---

## 3. JSON Schema Excerpt

```jsonc
{
  "$id": "https://api.example.com/schema/purchase-page.json",
  "type": "object",
  "properties": {
    "pageId":          { "type": "integer" },
    "pageTypeId":      { "type": "integer" },
    "startDate":       { "type": ["string","null"], "format": "date-time" },
    "endDate":         { "type": ["string","null"], "format": "date-time" },
    "resolveRequestId":{ "type": "string" },
    "options": {
      "type": "array",
      "items": { "$ref": "#/definitions/PageOption" }
    },
    "additionalData":  { "type": "object" }
  },
  "required": ["pageId","pageTypeId","options","resolveRequestId"]
}
```

---

## 4 Quick integration Checklist

1. **Authenticate** via the identity service to obtain a bearer token.  
2. Call **GET /v1/purchase-pages** once per session or when needed.  
3. **Cache** results locally; refresh when session, locale, or player segment changes.  
4. **Handle errors** according to the mapping table above.  
5. Add a unique **`X-Request-Id`** header for analytics and troubleshooting.

---

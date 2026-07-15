# Take Water Delivery API

OpenAPI 3.0 Swagger spec for the **Take Water** purified water delivery mobile app.

- **Stack:** ASP.NET Core (backend) → React Native (mobile)
- **Base URL (Staging):** `https://stg-api.takewater.com/api/v1`

---

## How to Use This Spec

### Option 1 — Swagger UI (Online)

1. Go to [editor.swagger.io](https://editor.swagger.io)
2. Click **File → Import File** → select `TakeWater.yaml`
3. The API docs render instantly — try any endpoint directly

### Option 2 — Swagger Editor (Local)

```bash
# install
npm install -g swagger-editor-dist

# or just open the file in VS Code with Swagger Viewer extension
```

### Option 3 — Postman

1. Open Postman → **Import** → **Link**
2. Paste: `https://editor.swagger.io`
3. Or import `TakeWater.yaml` directly via **File → Import**

### Option 4 — Insomnia

1. **File → Import** → **Choose Files** → select `TakeWater.yaml`
2. All endpoints appear as a request collection

---

## API Overview

### Servers

| Environment | URL |
|---|---|
| Local Dev | `http://localhost:5000/api/v1` |
| Staging | `https://stg-api.takewater.com/api/v1` |
| Production | `https://api.takewater.com/api/v1` |

### Authentication

All authenticated endpoints require a **JWT Bearer Token**:

```
Authorization: Bearer <your-token>
```

Get a token from `POST /login`.

---

## Endpoints

### Auth

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/register` | Customer registration |
| `POST` | `/login` | Customer login (returns JWT) |
| `POST` | `/logout` | Invalidate session |
| `POST` | `/forgot-password` | Two-step password reset |

### Products

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/products` | Product list (paginated, searchable) |

### Profile

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/profile` | Get customer profile |
| `PUT` | `/profile/update` | Update profile fields |

### Orders

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/orders` | Create new order |
| `GET` | `/orders/customers/{id}` | Order history (paginated, filterable) |
| `GET` | `/orders/{id}` | Order detail with line items |
| `PUT` | `/orders/{id}/cancel` | Cancel an order |

### Other

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/regions` | Regions with townships |
| `GET` | `/posts` | Knowledge sharing articles |
| `GET` | `/payment-methods` | Payment methods list |
| `GET` | `/privacy-policy` | Privacy policy content |

---

## Database Tables

| Table | Purpose |
|---|---|
| `m_customers` | Customer accounts |
| `m_staffs` | Staff accounts (admin/sales) |
| `m_products` | Product catalog with pricing |
| `m_units` | Product units (bottle, pack, etc.) |
| `m_regions` | Regions/States |
| `m_townships` | Townships/Districts |
| `t_orders` | Order headers |
| `t_order_details` | Order line items |
| `t_knowledge_posts` | Knowledge articles |
| `m_token` | Auth tokens (JWT) |

---

## Quick Start — Test the API

```bash
# 1. Register a new customer
curl -X POST https://stg-api.takewater.com/api/v1/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "mobile_phone": "09123456789",
    "password": "password123",
    "gender": 1
  }'

# 2. Login
curl -X POST https://stg-api.takewater.com/api/v1/login \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_phone": "09123456789",
    "password": "password123"
  }'

# 3. Get products (with token from step 2)
curl https://stg-api.takewater.com/api/v1/products \
  -H "Authorization: Bearer <your-token>"

# 4. Place an order
curl -X POST https://stg-api.takewater.com/api/v1/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your-token>" \
  -d '{
    "amount": 5000,
    "region_id": 1,
    "township_id": 3,
    "street_address": "အမှတ် ၁၂၄၊ ပြည်ထောင်စုလမ်း",
    "phone_no": "09123456789",
    "items": [
      { "product_id": 1, "price": 2500, "quantity": 2 }
    ]
  }'
```

---

## Need Help?

- **Repo:** https://github.com/thetzinsoe-cgm/take-water-api
- **Mobile App:** https://ais-pre-h35xyo5q4f5an2rej4ym6l-633477186186.asia-southeast1.run.app/

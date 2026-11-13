# Inventory API v2 reference

Base URL: https://api.example.com/v2

v1 will stop working on March 31, 2027. Move to v2 before then.

## Authentication

Send your API key as a Bearer token in the Authorization header:

Authorization: Bearer YOUR_API_KEY

API keys in query parameters are no longer accepted.

## Endpoints

### GET /v2/items

Lists the items in your inventory.

Query parameters:

| Name | Type | Required | Description |
|---|---|---|---|
| limit | integer | No | Items per page. Default: 20. Max: 100. |
| cursor | string | No | The next_cursor value from the previous response. Leave it out for the first page. |

Example response:

{
  "data": [
    {"id": "itm_204", "name": "Desk lamp", "quantity": 42, "warehouse_id": "wh_lagos_01"},
    {"id": "itm_205", "name": "Office chair", "quantity": 7, "warehouse_id": "wh_lagos_01"}
  ],
  "next_cursor": "eyJpZCI6Iml0bV8yMDUifQ"
}

next_cursor is null on the last page.

### GET /v2/items/{id}

Returns one item.

Response fields:

| Name | Type | Description |
|---|---|---|
| id | string | The item ID. |
| name | string | The item name. |
| quantity | integer | Units in stock. |
| warehouse_id | string | The warehouse that holds the item. New in v2. |

### POST /v2/items/{id}/adjust

Adds or removes stock for one item.

Request body:

| Name | Type | Required | Description |
|---|---|---|---|
| quantity | integer | Yes | Units to add. Use a negative number to remove units. |
| reason | string | No | Why the stock changed, such as "damaged" or "restock". New in v2. |

Example request:

curl -X POST "https://api.example.com/v2/items/itm_204/adjust" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"quantity": -5, "reason": "damaged"}'

## Errors

Errors return an object with a code and a message:

{"error": {"code": "item_not_found", "message": "Item not found"}}

| Status | Code | Meaning |
|---|---|---|
| 401 | invalid_api_key | Missing or invalid API key. |
| 404 | item_not_found | Item not found. |
| 429 | rate_limited | Too many requests. Wait the number of seconds in the Retry-After header. New in v2. |

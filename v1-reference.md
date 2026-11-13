# Inventory API v1 reference

Base URL: https://api.example.com/v1

## Authentication

Send your API key as a query parameter on every request:

GET /v1/items?api_key=YOUR_API_KEY

## Endpoints

### GET /v1/items

Lists the items in your inventory.

Query parameters:

| Name | Type | Required | Description |
|---|---|---|---|
| page | integer | No | Page number. Default: 1. |
| per_page | integer | No | Items per page. Default: 20. Max: 100. |

Example response:

{
  "data": [
    {"id": "itm_204", "name": "Desk lamp", "qty": 42},
    {"id": "itm_205", "name": "Office chair", "qty": 7}
  ],
  "page": 1,
  "per_page": 20,
  "total_pages": 5
}

### GET /v1/items/{id}

Returns one item.

Response fields:

| Name | Type | Description |
|---|---|---|
| id | string | The item ID. |
| name | string | The item name. |
| qty | integer | Units in stock. |

### POST /v1/items/{id}/adjust

Adds or removes stock for one item.

Request body:

| Name | Type | Required | Description |
|---|---|---|---|
| qty | integer | Yes | Units to add. Use a negative number to remove units. |

Example request:

curl -X POST "https://api.example.com/v1/items/itm_204/adjust?api_key=YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"qty": -5}'

## Errors

Errors return a message string:

{"error": "Item not found"}

| Status | Meaning |
|---|---|
| 401 | Missing or invalid API key. |
| 404 | Item not found. |

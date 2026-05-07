---
name: create-unlock-deal
description: Programatically create an Unlock Deal to assist engineers with local development
metadata:
  trigger_phrases:
    - "create deal"
    - "create unlock deal"
    - "create a new unlock deal"
---

# Instructions

If you find an error, stop and report it, don't try to create python environments/scripts or install dependencies.
Do not to troubleshoot technical issues.

## Step 1: Generate a random customer email and phone number
use the skill `generate-random-customer-email`

## Step 2: Pick a random state and address
use the skill `pick-random-usa-address`



## Step 3: Get Admin credentials
```sh
curl -X POST http://localhost:8000/api/v1/token-auth/ \
  -H 'Content-Type: application/json' \
  -d '{
    "username": "admin@example.com",
    "password": "Unlock123!"
}'
```

## Step 4: Signup the customer
- Do not pass admin `Authorization` header for this step
- With all the information gathered, signup the customer `POST http://localhost:8000/api/v1/customer/signup/`
- Required header: `Origin: http://localhost:3000`
```json
// Payload example
{
  "first_name": <first_name>,
  "last_name": <last_name>,
  "email": <email>,
  "phone": <phone>,
  "timezone": <timezone>,
  "is_manually_created": true
}
```

## Step 5: Create the deal
`POST http://localhost:8000/api/v2/application/`
- Required headers:
  - `Authorization: Bearer <admin_access_token>`
  - `Origin: http://localhost:3000`

```json
// Payload
{
  "is_new_user": true,
  "email": <email>,
  "property": {
    "address": {
      "street": <street>,
      "city": <city>,
      "state": <state>,
      "zip_code": <zip_code>,
      "statename": <statename>
    },
    "property_type": "single-family-detached",
    "property_usage": "principal-residence"
  },
  "use_of_proceeds": [],
  "manually_created": true
}
```

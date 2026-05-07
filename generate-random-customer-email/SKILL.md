---
name: generate-random-customer-email
description: Generate a random customer email address for testing purposes
metadata:
  trigger_phrases:
    - "generate random customer email"
---

# Instructions
This email address will be the customer email.

Generate a random email address with the following schema:
{first_name}.{last_name}.{timestamp}@example.com

# Return
```json
{
  "email": <generated_email_address>,
  "first_name": <generated_first_name>,
  "last_name": <generated_last_name>,
  "telephone": <random_us_phone_number> // Don't use hyphens
}
```
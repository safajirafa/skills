---
name: pick-random-usa-address
description: Pick a random USA address for testing purposes
metadata:
  trigger_phrases:
    - "pick random address"
---

# Instructions
- Backend is running via Docker.
- Stick to instructions.

## Step 1: 
Get into the backend container: `docker exec -it backend sh`

## Step 2: Get a list of states where Unlock operates
```sh
python manage.py shell -c "from api.models.state import State; import random, json; states = list(State.objects.filter(status='Originating').values_list('state_abbreviation', flat=True)); picked = random.choice(states) if states else None; print(json.dumps({'list_of_originating_states': states, 'randomly_picked_state': picked}, indent=2))"
```

## Step 3: Pick a random home address only in the state you picked in the previous step.
```sh
python manage.py shell -c " \
from api.services.core_logic_v2 import CoreLogicV2Client
import json

with CoreLogicV2Client() as client:
    addresses = client.find_addresses('Detroit, MI')
    if addresses:
        print(json.dumps(addresses[0], indent=2))
    else:
        print("No addresses found")
"
```

# Return
```json
{
  "address": "<street> <number>",
  "city": "<city>",
  "state": "<state>",
  "zip_code": "<zip_code>",
  "county": "<county>"
}
```
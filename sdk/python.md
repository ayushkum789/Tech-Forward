---
title: "Python SDK"
description: "Python SDK for backend services"
icon: "python"
---

## Installation

```bash
pip install api-client
```

## Quick Start

```python
from api_client import APIClient

# Initialize client
client = APIClient(api_key="YOUR_API_KEY")

# Get all resources
resources = client.resources.get_all()

# Get resource by ID
resource = client.resources.get(1)

# Create a resource
new_resource = client.resources.create({
    "name": "Sample Resource",
    "value": 100
})
```

## Error Handling

```python
from api_client.exceptions import NotFoundError, ValidationError

try:
    resource = client.resources.get(999)
except NotFoundError:
    print("Resource not found")
except ValidationError as e:
    print(f"Validation error: {e}")
```

## Type Safety

```python
from api_client.types import Resource

def process_resource(resource: Resource) -> None:
    print(f"Name: {resource.name}")
    print(f"Value: {resource.value}")
```

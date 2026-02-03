---
title: "Get Movie by ID"
openapi: "openapi.json GET /api/movies/{id}"
description: "Returns detailed information about a single movie."
---

Retrieve a specific movie by its unique identifier.

<ParamField path="id" type="integer" required placeholder="1">
  Unique movie identifier (minimum: 1)
</ParamField>

<ResponseExample>
```json 200 Success
{
  "id": 1,
  "title": "The Shawshank Redemption",
  "director": "Frank Darabont",
  "year": 1994,
  "genre": "Drama",
  "rating": 9.3
}
```

```json 404 Not Found
{
  "message": "Movie with id 999 not found",
  "statusCode": 404
}
```

```json 500 Internal Server Error
{
  "message": "An internal server error occurred",
  "statusCode": 500
}
```

</ResponseExample>

---
title: "Delete Movie"
api: "DELETE http://localhost:5065/api/movies/{id}"
description: "Permanently removes a movie from the database identified by its ID."
---

Delete a movie from the database. This operation is permanent and cannot be undone.

<ParamField path="id" type="integer" required placeholder="5">
  Movie ID to delete (minimum: 1)
</ParamField>

<ResponseExample>
```json 204 No Content
// No content returned. Status code 204 indicates successful deletion.
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

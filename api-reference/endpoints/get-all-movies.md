---
title: "Get All Movies"
api: "GET http://localhost:5065/api/movies"
description: "Returns a complete list of all movies in the database with their details including title, director, year, genre, and rating."
---

Returns an array of all movies in the database.

<ResponseExample>
```json 200 Success
[
  {
    "id": 1,
    "title": "The Shawshank Redemption",
    "director": "Frank Darabont",
    "year": 1994,
    "genre": "Drama",
    "rating": 9.3
  },
  {
    "id": 2,
    "title": "The Godfather",
    "director": "Francis Ford Coppola",
    "year": 1972,
    "genre": "Crime",
    "rating": 9.2
  },
  {
    "id": 3,
    "title": "The Dark Knight",
    "director": "Christopher Nolan",
    "year": 2008,
    "genre": "Action",
    "rating": 9.0
  }
]
```

```json 500 Internal Server Error
{
  "message": "An internal server error occurred",
  "statusCode": 500
}
```

</ResponseExample>

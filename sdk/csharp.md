---
title: "C# SDK"
description: "Official C#/.NET SDK for Movies API"
icon: "microsoft"
---

## Installation

Install via NuGet Package Manager or dotnet CLI:

<CodeGroup>

```bash dotnet CLI
dotnet add package MoviesApi.SDK
```

```bash Package Manager
Install-Package MoviesApi.SDK
```

```xml PackageReference
<PackageReference Include="MoviesApi.SDK" Version="1.0.0" />
```

</CodeGroup>

## Requirements

- .NET 6.0 or higher
- .NET Framework 4.7.2 or higher
- .NET Core 3.1 or higher

## Quick Start

```csharp
using MoviesApi.SDK;

// Initialize client
var client = new MoviesClient("YOUR_API_KEY");

// Use the client
var movies = await client.Movies.GetAllAsync();
Console.WriteLine($"Found {movies.Data.Count} movies");
```

## Client Configuration

### Basic Configuration

```csharp
using MoviesApi.SDK;

var client = new MoviesClient("YOUR_API_KEY");
```

### Advanced Configuration

```csharp
using MoviesApi.SDK;

var options = new ClientOptions
{
    BaseUrl = "https://api.moviesapi.com",
    Timeout = TimeSpan.FromSeconds(30),
    MaxRetries = 3,
    RetryDelay = TimeSpan.FromSeconds(1)
};

var client = new MoviesClient("YOUR_API_KEY", options);
```

### Configuration Options

```csharp
public class ClientOptions
{
    public string BaseUrl { get; set; } = "https://api.moviesapi.com";
    public TimeSpan Timeout { get; set; } = TimeSpan.FromSeconds(30);
    public int MaxRetries { get; set; } = 3;
    public TimeSpan RetryDelay { get; set; } = TimeSpan.FromSeconds(1);
    public Dictionary<string, string> Headers { get; set; } = new();
}
```

## Get All Movies

Retrieve a list of all movies with optional filtering and pagination.

```csharp
// Get all movies (default pagination)
var response = await client.Movies.GetAllAsync();

foreach (var movie in response.Data)
{
    Console.WriteLine($"{movie.Title} ({movie.Year}) - {movie.Rating}/10");
}

// With pagination
var page1 = await client.Movies.GetAllAsync(page: 1, limit: 20);

// With filters
var dramaMovies = await client.Movies.GetAllAsync(
    genre: "Drama",
    minRating: 8.0f,
    sort: "-rating"
);

// Multiple filters
var filtered = await client.Movies.GetAllAsync(
    genre: "Sci-Fi",
    year: 2010,
    director: "Nolan",
    minRating: 8.0f,
    maxRating: 9.0f,
    page: 1,
    limit: 10,
    sort: "-year,title"
);
```

### Response Type

```csharp
public class MoviesResponse
{
    public bool Success { get; set; }
    public List<Movie> Data { get; set; }
    public PaginationInfo Pagination { get; set; }
}

public class PaginationInfo
{
    public int Page { get; set; }
    public int Limit { get; set; }
    public int Total { get; set; }
    public int Pages { get; set; }
    public bool HasNext { get; set; }
    public bool HasPrev { get; set; }
}
```

## Get Movie by ID

Retrieve a specific movie by its ID.

```csharp
// Get movie by ID
var movie = await client.Movies.GetAsync(1);
Console.WriteLine($"Title: {movie.Title}");
Console.WriteLine($"Director: {movie.Director}");
Console.WriteLine($"Rating: {movie.Rating}/10");

// With error handling
try
{
    var movie = await client.Movies.GetAsync(999);
}
catch (MoviesApiException ex) when (ex.StatusCode == 404)
{
    Console.WriteLine("Movie not found");
}
```

### Response Type

```csharp
public class Movie
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Director { get; set; }
    public int Year { get; set; }
    public string Genre { get; set; }
    public float? Rating { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
}
```

## Create Movie

Add a new movie to the database.

```csharp
// Create a new movie
var newMovie = await client.Movies.CreateAsync(new CreateMovieRequest
{
    Title = "Inception",
    Director = "Christopher Nolan",
    Year = 2010,
    Genre = "Sci-Fi",
    Rating = 8.8f
});

Console.WriteLine($"Created movie with ID: {newMovie.Id}");

// Without rating (optional field)
var movie = await client.Movies.CreateAsync(new CreateMovieRequest
{
    Title = "Oppenheimer",
    Director = "Christopher Nolan",
    Year = 2023,
    Genre = "Biography"
});

// Using object initializer
var movie = await client.Movies.CreateAsync(new CreateMovieRequest
{
    Title = "The Matrix",
    Director = "The Wachowskis",
    Year = 1999,
    Genre = "Sci-Fi",
    Rating = 8.7f
});
```

### Input Type

```csharp
public class CreateMovieRequest
{
    [Required]
    [StringLength(200, MinimumLength = 1)]
    public string Title { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 1)]
    public string Director { get; set; }

    [Required]
    [Range(1900, 2100)]
    public int Year { get; set; }

    [Required]
    public string Genre { get; set; }

    [Range(0.0, 10.0)]
    public float? Rating { get; set; }
}
```

### Valid Genres

```csharp
public static class Genres
{
    public const string Action = "Action";
    public const string Adventure = "Adventure";
    public const string Animation = "Animation";
    public const string Biography = "Biography";
    public const string Comedy = "Comedy";
    public const string Crime = "Crime";
    public const string Documentary = "Documentary";
    public const string Drama = "Drama";
    public const string Fantasy = "Fantasy";
    public const string Horror = "Horror";
    public const string Musical = "Musical";
    public const string Mystery = "Mystery";
    public const string Romance = "Romance";
    public const string SciFi = "Sci-Fi";
    public const string Thriller = "Thriller";
    public const string Western = "Western";
}
```

## Update Movie

Update an existing movie's information.

```csharp
// Update single field
var updated = await client.Movies.UpdateAsync(3, new UpdateMovieRequest
{
    Rating = 9.0f
});

// Update multiple fields
var updated = await client.Movies.UpdateAsync(3, new UpdateMovieRequest
{
    Title = "Inception: Director's Cut",
    Rating = 9.0f,
    Year = 2010
});

// Using object initializer
var updated = await client.Movies.UpdateAsync(3, new UpdateMovieRequest
{
    Rating = 8.9f
    // Other fields remain unchanged
});
```

### Input Type

```csharp
public class UpdateMovieRequest
{
    [StringLength(200, MinimumLength = 1)]
    public string? Title { get; set; }

    [StringLength(100, MinimumLength = 1)]
    public string? Director { get; set; }

    [Range(1900, 2100)]
    public int? Year { get; set; }

    public string? Genre { get; set; }

    [Range(0.0, 10.0)]
    public float? Rating { get; set; }
}
```

## Delete Movie

Permanently delete a movie from the database.

```csharp
// Delete movie
await client.Movies.DeleteAsync(3);
Console.WriteLine("Movie deleted successfully");

// With confirmation
var movie = await client.Movies.GetAsync(3);
Console.Write($"Delete \"{movie.Title}\"? (y/n): ");
var response = Console.ReadLine();

if (response?.ToLower() == "y")
{
    await client.Movies.DeleteAsync(3);
    Console.WriteLine("Deleted");
}

// With error handling
try
{
    await client.Movies.DeleteAsync(999);
}
catch (MoviesApiException ex) when (ex.StatusCode == 404)
{
    Console.WriteLine("Movie already deleted or never existed");
}
```

## Pagination Helper

Iterate through all pages automatically:

```csharp
// Manual pagination
int page = 1;
bool hasMore = true;

while (hasMore)
{
    var response = await client.Movies.GetAllAsync(page: page, limit: 20);

    foreach (var movie in response.Data)
    {
        Console.WriteLine(movie.Title);
    }

    hasMore = response.Pagination.HasNext;
    page++;
}

// Using IAsyncEnumerable (C# 8.0+)
await foreach (var movie in client.Movies.IterateAsync())
{
    Console.WriteLine(movie.Title);
}
```

## Error Handling

The SDK provides typed exceptions for better error handling:

```csharp
using MoviesApi.SDK.Exceptions;

try
{
    var movie = await client.Movies.GetAsync(999);
}
catch (MoviesApiException ex)
{
    Console.WriteLine($"Error Code: {ex.Code}");
    Console.WriteLine($"Message: {ex.Message}");
    Console.WriteLine($"Status: {ex.StatusCode}");
    Console.WriteLine($"Field: {ex.Field}"); // For validation errors

    // Handle specific errors
    switch (ex.Code)
    {
        case "NOT_FOUND":
            Console.WriteLine("Movie does not exist");
            break;
        case "UNAUTHORIZED":
            Console.WriteLine("Invalid API key");
            break;
        case "RATE_LIMIT_EXCEEDED":
            Console.WriteLine($"Rate limit hit, retry after: {ex.RetryAfter} seconds");
            break;
        case "INVALID_PARAMETER":
            Console.WriteLine($"Invalid {ex.Field}: {ex.Message}");
            break;
        default:
            Console.WriteLine($"Unknown error: {ex.Message}");
            break;
    }
}
catch (HttpRequestException ex)
{
    // Network errors
    Console.WriteLine($"Request failed: {ex.Message}");
}
```

### Exception Types

```csharp
public class MoviesApiException : Exception
{
    public string Code { get; set; }
    public int StatusCode { get; set; }
    public string? Field { get; set; }
    public int? RetryAfter { get; set; }
}
```

## Dependency Injection

Integrate with ASP.NET Core dependency injection:

```csharp
// Startup.cs or Program.cs
using MoviesApi.SDK;

services.AddSingleton<IMoviesClient>(sp =>
{
    var configuration = sp.GetRequiredService<IConfiguration>();
    var apiKey = configuration["MoviesApi:ApiKey"];
    return new MoviesClient(apiKey);
});

// Usage in controller
public class MoviesController : ControllerBase
{
    private readonly IMoviesClient _moviesClient;

    public MoviesController(IMoviesClient moviesClient)
    {
        _moviesClient = moviesClient;
    }

    [HttpGet]
    public async Task<IActionResult> GetMovies()
    {
        var movies = await _moviesClient.Movies.GetAllAsync();
        return Ok(movies.Data);
    }
}
```

## Advanced Examples

### Retry Logic with Polly

```csharp
using Polly;
using Polly.Retry;

// Create retry policy
var retryPolicy = Policy
    .Handle<MoviesApiException>(ex => ex.StatusCode >= 500)
    .WaitAndRetryAsync(
        3,
        retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt))
    );

// Use with client
var movie = await retryPolicy.ExecuteAsync(async () =>
{
    return await client.Movies.GetAsync(1);
});
```

### Bulk Operations

```csharp
public async Task<List<Result>> BulkCreateMoviesAsync(List<CreateMovieRequest> movies)
{
    var results = new List<Result>();

    foreach (var movieData in movies)
    {
        try
        {
            var movie = await client.Movies.CreateAsync(movieData);
            results.Add(new Result { Success = true, Movie = movie });
        }
        catch (MoviesApiException ex)
        {
            results.Add(new Result
            {
                Success = false,
                Error = ex.Message,
                Data = movieData
            });
        }

        // Rate limiting: wait 100ms between requests
        await Task.Delay(100);
    }

    return results;
}
```

### Caching with IMemoryCache

```csharp
using Microsoft.Extensions.Caching.Memory;

public class CachedMoviesService
{
    private readonly IMoviesClient _client;
    private readonly IMemoryCache _cache;
    private readonly TimeSpan _cacheDuration = TimeSpan.FromMinutes(5);

    public CachedMoviesService(IMoviesClient client, IMemoryCache cache)
    {
        _client = client;
        _cache = cache;
    }

    public async Task<Movie> GetMovieAsync(int id)
    {
        var cacheKey = $"movie_{id}";

        if (_cache.TryGetValue(cacheKey, out Movie cachedMovie))
        {
            return cachedMovie;
        }

        var movie = await _client.Movies.GetAsync(id);

        _cache.Set(cacheKey, movie, _cacheDuration);

        return movie;
    }
}
```

## Best Practices

<AccordionGroup>
  <Accordion title="Use Configuration" icon="gear">
    ```csharp
    // appsettings.json
    {
      "MoviesApi": {
        "ApiKey": "YOUR_API_KEY",
        "BaseUrl": "https://api.moviesapi.com"
      }
    }
    
    // Startup.cs
    var apiKey = configuration["MoviesApi:ApiKey"];
    var client = new MoviesClient(apiKey);
    ```
  </Accordion>
  
  <Accordion title="Dispose Clients Properly" icon="trash">
    ```csharp
    // Use using statement
    using var client = new MoviesClient("YOUR_API_KEY");
    var movies = await client.Movies.GetAllAsync();
    
    // Or with DI, register as singleton
    services.AddSingleton<IMoviesClient>(...);
    ```
  </Accordion>
  
  <Accordion title="Handle Exceptions" icon="triangle-exclamation">
    ```csharp
    try
    {
        var movie = await client.Movies.GetAsync(id);
    }
    catch (MoviesApiException ex)
    {
        _logger.LogError(ex, "API error: {Code}", ex.Code);
        // Handle API errors
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Unexpected error");
        // Handle other errors
    }
    ```
  </Accordion>
</AccordionGroup>

## Next Steps

<CardGroup cols={2}>
  <Card title="API Reference" icon="book" href="/api-reference/introduction">
    Explore all available endpoints
  </Card>
  <Card title="JavaScript SDK" icon="js" href="/sdk/javascript">
    Check out the JavaScript SDK
  </Card>
  <Card title="Python SDK" icon="python" href="/sdk/python">
    Check out the Python SDK
  </Card>
  <Card title="GitHub" icon="github" href="https://github.com/moviesapi/sdk-csharp">
    View source code
  </Card>
</CardGroup>

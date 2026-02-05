---
title: "C# SDK"
description: ".NET SDK for enterprise applications"
icon: "microsoft"
---

## Installation

```bash
dotnet add package API.SDK
```

## Quick Start

```csharp
using API.SDK;

// Initialize client
var client = new APIClient("YOUR_API_KEY");

// Get all resources
var resources = await client.Resources.GetAllAsync();

// Get resource by ID
var resource = await client.Resources.GetAsync(1);

// Create a resource
var newResource = await client.Resources.CreateAsync(new CreateResourceRequest
{
    Name = "Sample Resource",
    Value = 100
});
```

## Configuration

```csharp
var options = new ClientOptions
{
    BaseUrl = "https://api.example.com",
    Timeout = TimeSpan.FromSeconds(30)
};

var client = new APIClient("YOUR_API_KEY", options);
```

## Error Handling

```csharp
using API.SDK.Exceptions;

try
{
    var resource = await client.Resources.GetAsync(999);
}
catch (NotFoundException)
{
    Console.WriteLine("Resource not found");
}
catch (ValidationException ex)
{
    Console.WriteLine($"Validation error: {ex.Message}");
}
```

## Strongly Typed Models

```csharp
public class Resource
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Value { get; set; }
}
```

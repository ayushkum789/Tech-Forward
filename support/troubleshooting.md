---
title: "Troubleshooting"
description: "Common issues and solutions"
---

# Troubleshooting Guide

## Common Issues

### CORS Errors

**Problem**: Browser shows CORS policy errors

**Solution**:

```csharp
// In your backend Program.cs
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowDocs", policy =>
    {
        policy.WithOrigins("http://localhost:3000")
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});
```

### Connection Refused

**Problem**: Cannot connect to API

**Solutions**:

- Verify API is running: `netstat -ano | findstr :5065`
- Check firewall settings
- Ensure correct port in baseUrl

### Invalid URL Errors

**Problem**: API playground shows "invalid URL"

**Solutions**:

- Check API path in endpoint frontmatter
- Verify baseUrl in mint.json
- Remove authentication headers if not required

### Timeout Errors

**Problem**: Requests timing out

**Solutions**:

- Increase timeout settings
- Check network connectivity
- Verify API is responding: `curl http://localhost:5065/movies`

## API Issues

### 404 Not Found

**Causes**:

- Incorrect endpoint URL
- Movie ID doesn't exist
- API route not registered

### 400 Bad Request

**Causes**:

- Missing required fields
- Invalid data types
- Validation errors

**Check**:

- Review request body format
- Verify all required fields
- Check data type constraints

### 500 Internal Server Error

**Actions**:

1. Check API server logs
2. Verify database connection
3. Review error handling middleware

## SDK Issues

### Module Not Found

```bash
# JavaScript
npm install movies-api-client

# C#
dotnet add package MoviesApiClient
```

### Type Errors

Ensure you're using the latest SDK version with proper type definitions.

## Performance Issues

### Slow Response Times

- Enable pagination for large datasets
- Add database indexes
- Use caching
- Optimize queries

## Getting Help

If you continue experiencing issues:

1. Check the [Error Reference](/support/errors)
2. Review API logs
3. Contact support with error details

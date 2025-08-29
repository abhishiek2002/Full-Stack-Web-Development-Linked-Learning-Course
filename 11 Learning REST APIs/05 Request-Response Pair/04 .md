# Exploring Response Headers

Response headers are key-value pairs the server sends back with every HTTP response. They contain information about the server, the response format, caching instructions, security policies, and much more. The Response panel populated by the VS Code REST Client displays the entire response from the REST API, including all headers and the response data. 

## Viewing response headers in VS Code

Let's examine the headers returned by the REST API.

1. From the `requests.http` file, find a `GET` request to the `/users` endpoint:

```
GET {{baseURL}}/users
```

1. Select "Send Request".
2. In the Response panel, the headers appear first with the body below.
3. Scroll through the Headers section to see all the information the server is providing.

*Note: Because you're using GitHub Codespaces, you will see a very long `Set-Cookie` header that you'll need to scroll past to see other headers.*

## Common response headers explained

Here are the most important headers you'll encounter when working with this REST API:

**Content-Type** - Tells you what format the response data is in:

```
Content-Type: application/json; charset=utf-8
```

This confirms the API is returning JSON data with UTF-8 encoding.

**Content-Length** - The size of the response body in bytes:

```
Content-Length: 156
```

This is useful for understanding the size of the data being transferred.

**Date** - When the server generated the response:

```
Date: Thu, 27 Jun 2025 14:30:25 GMT
```

**Server** - Information about the web server:

```
Server: uvicorn
```

This tells us the API is running on Uvicorn, which is a common ASGI server for FastAPI applications.

## Status-specific headers

Different types of responses include different headers. Let's explore what you'll see with various operations.

**Successful GET request** (200 OK):

```json
GET {{baseURL}}/users/987654
Response headers will include:

Content-Type: application/json
Content-Length: [size]
Standard server headers
```

**Successful POST request** (201 Created or 200 OK):

```json
POST {{baseURL}}/users
Content-Type: application/json

{
  "name": "Test User",
  "email": "test@example.com",
  "address": "123 Test St",
  "phoneNumber": "5551234567"
}
```

Response headers are similar to `GET`, but the status code will be different.

**Error responses** (404 Not Found, 400 Bad Request):

```
GET {{baseURL}}/users/999999
```

Even error responses include the same basic headers, but you'll see the error status code reflected in the response.

## FastAPI-specific headers

Since this REST API is built with FastAPI, you may see some framework-specific headers:

### Access-Control headers

 - These relate to CORS (Cross-Origin Resource Sharing):

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, PATCH
Access-Control-Allow-Headers: Content-Type
```

These headers tell browsers which domains can access the API and what methods are allowed.

## Headers to pay attention to

When working with REST APIs, these headers deserve special attention.

### Content-Type in responses:

- `application/json` for JSON data (this is almost always what you want)
- `text/html` might indicate an error page instead of API data

### Status information:

The HTTP status code (200, 404, 500, etc.) is displayed with the headers and can be used to handle content and errors cleanly.

### Content-Length:

This helps you understand if you're getting the amount of data you expect.

- Very small responses might indicate errors
- Very large responses might indicate you need pagination or that you need to be more specific in your query
- Very large responses can also help explain slow response times

## Comparing different request types

Try sending different types of requests and compare their headers:

```
# GET request
GET {{baseURL}}/users

###

# POST request 
POST {{baseURL}}/users
Content-Type: application/json

{
  "name": "Header Test",
  "email": "headers@example.com",
  "address": "456 Header Ave",
  "phoneNumber": "5559998877"
}

###

# Error request (non-existent user)
GET {{baseURL}}/users/999999
```

Note how the headers remain largely consistent across different request types, but the status codes and content length will vary.

Bottom line: response headers are worth paying attention to and understanding because they help you see what’s happening when you interact with a REST API, and help you figure out what’s going wrong when things go wrong, which is an inevitability when working with these tools!
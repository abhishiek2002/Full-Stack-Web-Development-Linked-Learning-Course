# Exploring POST Requests
GET requests typically access REST API endpoints directly without providing further request data. POST requests are a different matter because now you’re sending data to the REST API and that data has to be contained within your request. This data is typically formatted as JSON and appended to the request body.

## Creating a new user 
Let's start by creating a new user. In this REST API, POST requests require both a URL endpoint and a request body. 

1. In the `requests.http` file, add a new line below the GET request, add ### to indicate a new request, and a new empty line below that. This way, you can have multiple requests in one file.
2. Add the following request:

```
POST {{baseURL}}/users
Content-Type: application/json
{
  "name": "Jane Smith",

  "email": "jane@example.com",

  "address": "456 Oak Ave",

  "phoneNumber": "5556667777"

}
```

1. Select “Send Request”.
2. In the Response panel you’ll see the following: `{ "message": "User created with ID 987655" }`.

Notice the key differences from the GET request:

- The Content-Type: application/json header tells the API what format you're sending 
- There's a blank line between the headers and the request body 
- The JSON data contains all the required fields for creating a user

Now you can request the data on this new user by sending a GET request to /users using the new user ID.

## POST endpoints in the REST API

The REST API has several POST endpoints, each with their own unique request bodies. You can see all the available POST endpoints listed in the FastAPI UI. For reference, they are:

POST /users - Create new user

Request body:

```
{

  "name": "string",

  "email": "string",

  "address": "string",

  "phoneNumber": "string"

}
```

POST /books - Create new book

Request body:

```
{

  "isbn": int,

  "title": "string",

  "author": "string",

  "publisher": "string",

  "year": int,

  "pages": int,

  "genre": "string",

  "location": "string"

}
```

POST /checkout - Checkout a book

Request body:

```
{

  "userId": int,

  "isbn": int

}
```

## Passing data in a URL query

In some cases, REST APIs require you to pass data in a URL query. The /return endpoint provides an example of this. To mark a book as returned, you access the endpoint for the book using its ISBN number and then add a URL query to the end of the request with the user ID of the person currently borrowing the book:

```
POST {{baseURL}}/return/9780743273565?user_id=987654
```

In response, you’ll get `{ "message": "Book returned successfully" }`.
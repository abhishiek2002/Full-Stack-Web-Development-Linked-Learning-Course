# REST vs. GraphQL

Before we continue, let’s take a quick side-step to compare REST to a newer and somewhat complimentary alternative: [GraphQL](https://graphql.org/).

GraphQL (short for Graph Query Language) was created by Facebook to provide a more nimble and bandwidth-aware alternative to REST, and it was open sourced in 2015. Since then, it has become a popular choice for data-driven web applications, in particular JavaScript-driven front-end frameworks with back-end data sources.

The core differences between REST and GraphQL are how you access data, and what data is returned.:

In REST, your data selection is defined by what endpoint you access, and a REST API typically has multiple endpoints for different levels of data granularity. When you access an endpoint, all the data at that endpoint is returned. Optionally, the REST API can allow filtering of that data for further specificity, but in all cases, the response from a REST API will match the form and structure of the data at the resource.

In GraphQL, your data selection is defined by the query you send to a single endpoint in your request. The GraphQL API uses your query to find the relevant data and returns only the specified data and nodes, structured to match your original query. This is how GraphQL is bandwidth-aware.: When used correctly, you only get the data you requested in return, and nothing else!

This all makes more sense with a practical example.:

Imagine two different library systems: A REST library where books are organized in sections based on format, category, author, etc., and the librarian needs to visit each section separately to gather what you need;, and a GraphQL library where all the books are in a large warehouse, and the librarian controls a system by that ingests your entire reading list and returns exactly what you asked for in one trip, including only the specific pages you asked for if your query is that specific.

Both these types of libraries exist, and in many cases both systems exist within a single library. Let’s see how they would be represented in code:

## A Simple Request

I can’t remember how many books I currently have out on loan from the library, and I want to use their API to find out. In the REST library, I need to make several requests: 

First, I send a request to the users endpoint with my ID to get my user profile which contains a list of current books I have checked out.

```powershell
GET /users/987654
```

The REST API responds with my entire user profile.

```json
{
  "userId": 987654,
  "name": "Morten Rand-Hendriksen",
  "email": "morten@example.com",
  "memberSince": "2006-04-15",
  "checkedOutBooks": [9780743273565, 9780452284234, 9780061120084],
  "fineBalance": 0,
  "address": "5432 Street",
  "phoneNumber": "5555555555"
}
```

Next, I send separate requests using the ISBN numbers for each of the books to get their info.

```powershell
GET /books/9780743273565
GET /books/9780452284234
GET /books/9780061120084
```

This gives me three separate JSON objects.

```json
{
  "isbn": 9780743273565,
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "publisher": "Scribner",
  "year": 1925,
  "pages": 180,
  "genre": "Fiction",
  "location": "3rd Floor, A-12"
}

{
  "isbn": 9780452284234,
  "title": "1984",
  "author": "George Orwell",
  "publisher": "Signet Classic",
  "year": 1949,
  "pages": 328,
  "genre": "Dystopian Fiction",
  "location": "3rd Floor, B-07"
}

{
  "isbn": 9780061120084,
  "title": "To Kill a Mockingbird",
  "author": "Harper Lee",
  "publisher": "Harper Perennial",
  "year": 1960,
  "pages": 324,
  "genre": "Fiction",
  "location": "3rd Floor, A-15"
}
```

This demonstrates how the relative rigidity of a REST API often results in way more data than we actually need.

In contrast, here is that same request packaged as a GraphQL query, where I not only specify what to search for (the user ID) but also what data I want in return and how it should be structured.

```GraphQL
query {
  user(id: 987654) {
    name
    checkedOutBooks {
      isbn
      title
      author
      dueDate
    }
  }
}
```

In response, I get a JSON object with only the requested data.

```json
{
  "data": {
    "user": {
      "name": "Morten Rand-Hendriksen",
      "checkedOutBooks": [
        {
          "isbn": 9780743273565,
          "title": "The Great Gatsby",
          "author": "F. Scott Fitzgerald",
          "dueDate": "2024-04-01"
        },
        {
          "isbn": 9780452284234,
          "title": "1984",
          "author": "George Orwell",
          "dueDate": "2024-04-05"
        },
        {
          "isbn": 9780061120084,
          "title": "To Kill a Mockingbird",
          "author": "Harper Lee",
          "dueDate": "2024-04-03"
        }
      ]
    }
  }
}
```

## Which is better? REST or GraphQL?

Looking at the above example, you might think GraphQL is leaving REST in the dust. Not so. REST and GraphQL solve different problems for different use cases, and when new services like AI APIs come online, they pretty much always start out with a REST API, with GraphQL as a possible future add-on. As a result, REST remains the standard because of its easy implementation, broad support, and cacheability. GraphQL provides speed, efficiency, and reduced data loads for data-heavy applications like mobile and dynamic web apps.
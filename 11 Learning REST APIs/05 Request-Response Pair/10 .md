# Exploring DELETE Requests

DELETE requests do exactly what they say: remove resources from the REST API. DELETE is the most destructive of all the operations, as it permanently removes everything including the ID from the system, so this operation should be used with extreme caution. For this reason many applications implement the REST API in such a way that when a user uses a “Delete” function in the app, it actually triggers a PUT or PATCH operation that sets a status field to archived or similar so the record is not permanently removed. It is also common for REST APIs to include safety checks to prevent accidental deletion of resources that are still in use.

## Understanding DELETE operations

DELETE requests are straightforward: Target the resource using the same identifier as GET (in this case, user ID or book ISBN) and remove it from the system. However, this REST API implements some additional protections to ensure deletions that cause side effects:

- Users cannot be deleted if they have active book checkouts
- Books cannot be deleted if they are currently checked out

## Deleting a user

Let's delete a user from the system. To start, make sure the user has no books on loan by querying their user data in a GET request. The user with the ID 555555 in the original data set has no books by default, so if you have not changed their status, you can use that ID:

1. In the requests.http file, add a new line below your previous requests, add ### to indicate a new request, and a new empty line below that.
2. Add the following request:
 
```
DELETE {{baseURL}}/users/555555
```

1. Select "Send Request".
2. If the user has no active checkouts, you'll see: `{ "message": "User 555555 deleted successfully" }`.

*Note: DELETE requests do not require a Content-Type header or request body because you're simply telling the API which resource to remove.*

## Handling DELETE errors

Because of the logic in the back end of the REST API, when something goes wrong it will return different HTTP status codes depending on the situation:

**404 Not Found** – The resource doesn't exist:

```
{ "detail": "User not found" }
```

```
{ "detail": "Book not found" }
```

**400 Bad Request** – The resource cannot be deleted due to system rules:

```
{ "detail": "Cannot delete user with active book checkouts. Please return all books first." }
```

```
{ "detail": "Cannot delete book that is currently checked out" }
```

## DELETE endpoints in the REST API

The REST API supports DELETE operations for both users and books. You can see all available endpoints in the FastAPI UI. For reference they are:

**`DELETE /users/{user_id}` – Delete a user**

No request body required. The user will only be deleted if they have no active book checkouts.

**`DELETE /books/{isbn}` – Delete a book**

No request body required. The book will only be deleted if it is not currently checked out.

## Best practices for DELETE operations

Like I explained earlier, DELETE is a destructive operation, so here are some best-practices to guide you when working with DELETE requests:

### Always verify before deleting.

In this REST API, that means checking what resources depend on the item you want to delete.

- Use GET /users/{user_id} to see if a user has checked out books
- Use GET /books/{isbn} to verify a book exists before attempting to delete it

### Handle errors gracefully.

DELETE operations may fail due to system logic.

- Always check the response status and handle errors appropriately
- The API provides clear error messages explaining why a deletion failed

Consider the order of operations:

If you need to delete a user with active checkouts:

1. Return their books using POST /return/{isbn}?user_id={user_id}.
2. Once all books are returned, delete the user.
# Task Management API Documentation

<h3>Overview</h3>

The Task Management API allows developers to interact with a task management system. This API provides endpoints for creating, reading, updating, and deleting tasks, and it also supports user management and task categorization.

<h2></h2>

<h3>Base URL</h3>

All API requests should be made to the following base URL:

```

https://api.taskmanager.com/v1

```

<h3>Authentication</h3>

The API uses OAuth 2.0 for authentication. You'll need to obtain a valid access token to make requests.
- Authentication method: Bearer Token
- Obtain Access Token: Follow the OAuth 2.0 authorization flow to get a valid access token

All requests to the API require an Authorization header:

```

Authorization: Bearer {access_token}

```

<h2></h2>

<h3>Rate Limiting</h3>

To ensure fair usage of the API, rate limits are enforced. Each user has a maximum of 1000 requests per hour.
- Rate Limit: 1000 requests per hour
- Rate Limit Response Headers:
- ```X-RateLimit-Limit```: The maximum number of requests allowed
- ```X-RateLimit-Remaining```: The number of requests remaining
- ```X-RateLimit-Reset```: The time (in UNIX timestamp format) when the rate limit will reset

<h2></h2>

<h3>Error Handling</h3>

The API uses standard HTTP status codes to indicate the success or failure of a request:
- ```200 OK```: The request was successful
- ```201 Created```: The resource was successfully created
- ```400 Bad Request```: The request is malformed or missing required parameters
- ```401 Unauthorized```: Authentication failed or access token is missing/expired
- ```403 Forbidden```: The user does not have permission to access the requested resource
- ```404 Not Found```: The requested resource does not exist
- ```500 Internal Server Error```: An unexpected error occurred on the server side

Error responses will include the following fields:
- ```error```: A brief description of the error
- ```message```: A detailed message explaining the error

Example of an error response:

```

{
  "error": "invalid_request",
  "message": "The 'due_date' parameter is missing."
}

```

<h2></h2>
  
<h3>Endpoints</h3>

<h4>1. Create a New Task</h4>

Create a new task with the specified parameters.

- Endpoint: ```/tasks```
- Method: ```POST```
- Description: This endpoint creates a new task in the system
- Request Body:

```

{
  "title": "Buy Groceries",
  "description": "Milk, Eggs, Bread",
  "due_date": "2025-02-05T18:00:00Z",
  "priority": "High",
  "status": "Pending",
  "category_id": "3"
}

```

- Parameters:
  - ```title``` (string): The title of the task (required)
  - ```description``` (string): A detailed description of the task (optional)
  - ```due_date``` (string, ISO 8601 format): The due date and time for the task (optional)
  - ```priority``` (string): The priority level of the task. Valid values: ```Low```, ```Medium```, ```High``` (optional)
  - ```status``` (string): The current status of the task. Valid values: ```Pending```, ```In Progress```, ```Completed``` (optional)
  - ```category_id``` (string): The ID of the category under which the task falls (optional)

- Response:

```

{
  "task_id": "12345",
  "title": "Buy Groceries",
  "description": "Milk, Eggs, Bread",
  "due_date": "2025-02-05T18:00:00Z",
  "priority": "High",
  "status": "Pending",
  "category_id": "3"
}

```

- Status Codes:
  - ```201 Created```: Task created successfully
  - ```400 Bad Request```: Missing or invalid parameters

<h2>
  
<h4>2. Get Task by ID</h4>

- Retrieve a specific task by its ID
- Endpoint: ```/tasks/{task_id}```
- Method: ```GET```
- Description: Fetch a task by its unique ID
- Parameters:
  - ```task_id``` (string): The ID of the task to retrieve (required)
- Response:

```

{
  "task_id": "12345",
  "title": "Buy Groceries",
  "description": "Milk, Eggs, Bread",
  "due_date": "2025-02-05T18:00:00Z",
  "priority": "High",
  "status": "Pending",
  "category_id": "3"
}

```

- Status Codes:
  - ```200 OK```: Task retrieved successfully.
  - ```404 Not Found```: Task not found.

<h2>
  
<h4>3. Update a Task</h4>

Update the details of an existing task.
- Endpoint: ```/tasks/{task_id}```
- Method: ```PUT```
- Description: Update the task with the specified ID.
- Parameters:
  - ```task_id``` (string): The ID of the task to update (required).
- Request Body:

```

{
  "title": "Buy Groceries",
  "description": "Milk, Eggs, Bread, Butter",
  "due_date": "2025-02-06T18:00:00Z",
  "priority": "Medium",
  "status": "In Progress"
}

```

- Response:

```

{
  "task_id": "12345",
  "title": "Buy Groceries",
  "description": "Milk, Eggs, Bread, Butter",
  "due_date": "2025-02-06T18:00:00Z",
  "priority": "Medium",
  "status": "In Progress"
}

```

- Status Codes:
  - ```200 OK```: Task updated successfully.
  - ```400 Bad Request```: Invalid data.
  - ```404 Not Found```: Task not found.

<h2></h2>
  
<h4>4. Delete a Task</h4>

Delete an existing task.
- Endpoint: ```/tasks/{task_id}```
- Method: ```DELETE```
- Description: Remove the task with the specified ID from the system.
- Parameters:
  - ```task_id``` (string): The ID of the task to delete (required).
- Response:

```

{
  "message": "Task deleted successfully."
}

```

- Status Codes:
  - ```200 OK```: Task deleted successfully.
  - ```404 Not Found```: Task not found.

<h2></h2>
  
<h4>5. Get All Tasks</h4>

Retrieve a list of all tasks.
- Endpoint: ```/tasks```
- Method: ```GET```
- Description: Get all tasks in the system, with optional filters for status, priority, and category.
- Parameters:
  - ```status``` (string, optional): Filter tasks by status (```Pending```, ```In Progress```, ```Completed```).
  - ```priority``` (string, optional): Filter tasks by priority (```Low```, ```Medium```, ```High```).
  - ```category_id``` (string, optional): Filter tasks by category ID.
- Response:

```

[
  {
    "task_id": "12345",
    "title": "Buy Groceries",
    "description": "Milk, Eggs, Bread",
    "due_date": "2025-02-05T18:00:00Z",
    "priority": "High",
    "status": "Pending",
    "category_id": "3"
  },
  {
    "task_id": "12346",
    "title": "Complete Homework",
    "description": "Math and Science assignments",
    "due_date": "2025-02-06T15:00:00Z",
    "priority": "Medium",
    "status": "In Progress",
    "category_id": "1"
  }
]

```

- Status Codes:
  - ```200 OK```: Tasks retrieved successfully.

<h2>Tutorials</h2>

<h3>How to Create a Task</h3>

- Get your access token by following the OAuth 2.0 flow
- Make a POST request to ```/tasks``` with the required data:
  - ```title```
  - ```description (optional)```
  - ```due_date (optional```)
- The response will contain the task details, including the ```task_id```

<h3>How to Update a Task</h3>

- Get the task ID by querying an existing task using ```/tasks/{task_id}```
- Make a PUT request to ```/tasks/{task_id}``` with the new data you want to update
- The task will be updated, and the response will reflect the new data

<h2>Conclusion</h2>

This API documentation provides all the necessary information to interact with the Task Management API effectively. By following the instructions and making appropriate requests, developers can easily integrate task management functionality into their applications.

<h2></h2>
<p align="center">
  <a href="https://github.com/rlangc"><b>Return to Home</b></a>

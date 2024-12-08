# Muval Laravel Test

## Concerns

- The overall look of the project feels a bit plain.
- Any user can access the tasks that are created by different users.
- **Create new task encountered issues/possible issues:**
  - Not able to create a new task due to an encountered 500 internal server error: “Field 'status' doesn't have a default value”.
  - Task store has no validation.
  - No API resources for handling data transformation.
  - Raw SQL query issues.
  - Error handling missing.
- **Edit task encountered issues:**
  - Echoing “Task Not Found”.
  - Capitalized “POST”.
  - Centralized login, registration, and task validation form request.
  - Centralized service for user registration and task management.
- Breakdown of Changes Made to Meet New API Requirements.
  
### Solution and Explanation

#### 1. **The overall look of the project feels a bit plain.**

- **What is wrong?**
  - The project feels visually unappealing.
  
- **Why is it a problem?**
  - A plain interface creates a less engaging and visually appealing experience for users.
  
- **How you would fix it:**
  - Applied Tailwind CSS to improve the visual design.

#### 2. **Any user can access the tasks created by different users.**

- **What is wrong?**
  - Logged-in users can access all tasks stored in the database, regardless of the owner.
  
- **Why is it a problem?**
  - Users can view tasks that do not belong to them, which could lead to privacy concerns and unauthorized access.
  
- **How you would fix it:**
  - Add a condition so that only the user who created a task can access it.

#### 3. **Create new task encountered issues/possible issues:**

##### 3.1. **500 internal server error: “Field 'status' doesn't have a default value.”**

- **What is wrong?**
  - The 'status' field in the database doesn't have a default value, causing a 500 error when creating a new task.
  
- **Why is it a problem?**
  - Users can't create new tasks, breaking the functionality of the application.
  
- **How you would fix it:**
  - Ensure that the `status` is included in the POST request.
  - Include the status field in the payload data to be stored in the database.
  - Create a `TaskService` to handle the logic for managing tasks.

##### 3.2. **Task store has no validation.**

- **What is wrong?**
  - No validation for each request field data.
  
- **Why is it a problem?**
  - It can store data even if the field has an empty string or invalid data.
  
- **How you would fix it:**
  - Create a store validation form request and implement it in the store and update functions.  
    *Command used:*  
    `php artisan make:request StoreTaskRequest`

#### 4. **No API resources for data transformation.**

- **What is wrong?**
  - There are no API resources to handle data transformation, meaning the data isn't properly formatted before being sent to the client.
  
- **Why is it a problem?**
  - The data may be inconsistent or difficult to work with on the client-side.
  
- **How you would fix it:**
  - Create resources and collections to handle the data transformation.
    *Command used to create resources:*  
    `php artisan make:resource TaskResource`  
    In the `TaskResource`, I included key information like `id`, `user`, `title`, `description`, `status`, `created_at`, and `updated_at` to ensure proper formatting.
  
    *Command used to create collections:*  
    `php artisan make:resource TaskCollection --collection`

#### 5. **Raw Query Issues.**

- **What is wrong?**
  - Raw queries are prone to SQL injection, which can compromise the security of the application.
  
- **Why is it a problem?**
  - SQL injection can allow attackers to manipulate the database, access sensitive data, or compromise the entire system.
  
- **How you would fix it:**
  - Convert raw queries into Eloquent to prevent SQL injection, ensuring cleaner and more readable code.

#### 6. **Error Handling Issues.**

- **What is wrong?**
  - Errors are displayed directly on the UI, which may expose sensitive details to the user.
  
- **Why is it a problem?**
  - Raw error messages on the UI confuse users, expose sensitive system information, and can lead to security vulnerabilities.
  
- **How you would fix it:**
  - Added `try` and `catch` to handle errors and display user-friendly messages, ensuring no sensitive details are exposed.

#### 7. **Edit task encountered issues.**

##### 7.1. **Echoing “Task Not Found”.**

- **What is wrong?**
  - Echoing 'Task Not Found' is not user-friendly and doesn’t follow proper HTTP response standards for handling missing resources.
  
- **Why is it a problem?**
  - It provides a poor user experience and does not return the correct 404 status code for a missing resource.
  
- **How you would fix it:**
  - Replaced the echo with `Task::findOrFail($id)`, which automatically throws a 404 Not Found error when there is no result.

##### 7.2. **Capitalized “POST”.**

- **What is wrong?**
  - Inconsistent capitalization for HTTP methods (e.g., 'POST' instead of 'post') leads to confusion.
  
- **Why is it a problem?**
  - It creates inconsistency in the codebase and can make it harder to maintain.
  
- **How you would fix it:**
  - Changed 'POST' to 'post' to ensure all HTTP methods are consistently lowercase.

#### 8. **Centralized login, registration, and task validation form request.**

- **What is wrong?**
  - No centralized validation for login, registration, and task forms, leading to inconsistent validation logic.
  
- **Why is it a problem?**
  - It results in duplicated or inconsistent validation logic, which can cause errors and make the application harder to maintain.
  
- **How you would fix it:**
  - Implemented a centralized login validation using a custom form request class, ensuring consistent validation rules across the application.

#### 9. **Centralized service for user registration and task management.**

- **What is wrong?**
  - No centralized service for user registration and task management, leading to scattered and duplicated logic.
  
- **Why is it a problem?**
  - The logic becomes harder to manage, leading to inconsistent behavior and potential errors.
  
- **How you would fix it:**
  - Created individual services for user registration and task management, which improved readability, maintainability, and scalability. These services can be easily reused across both web and API requests.

#### 10. Breakdown of Changes Made to Meet New API Requirements.:

- Created centralized individual services for user registration and task management.
- Ensured services can be utilized for both web and API requests.
- Created new controllers to handle request payloads.
- Ensured controllers only handle HTTP requests and pass logic to respective services.
- Created API routes to handle endpoints.

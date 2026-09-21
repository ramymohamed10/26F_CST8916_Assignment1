# CST8916 – Remote Data and Real-time Applications

## Assignment 1: REST API Extension

|                  |                             |
| ---------------- | --------------------------- |
| **Semester**     | Fall 2026                   |
| **Release Date** | September 25, 2026 (Week 3) |
| **Due Date**     | October 2, 2026 at 11:59 PM |
| **Weight**       | 10% of final grade          |
| **Submission**   | Brightspace                 |

---

## Table of Contents

1. [Overview](#1-overview)
2. [Learning Objectives](#2-learning-objectives)
3. [Requirements](#3-requirements)
4. [Submission Instructions](#4-submission-instructions)
5. [Marking Rubric](#5-marking-rubric)
6. [Academic Integrity](#6-academic-integrity)

---

## 1. Overview

In the Week 2 lab, you worked with a Flask REST API that performs CRUD operations on a `users` resource. In this assignment, you will extend that API by adding a second resource (`tasks`) that has a relationship with users.

This simulates a real-world scenario where APIs manage multiple related resources. You will also deploy your API to Azure App Service and demonstrate its functionality through a video recording.

---

## 2. Learning Objectives

Upon successful completion of this assignment, you will be able to:

| #   | Objective                                                       |
| --- | --------------------------------------------------------------- |
| 1   | Demonstrate understanding of RESTful API design principles      |
| 2   | Extend an existing Flask API with new functionality             |
| 3   | Implement proper HTTP methods, status codes, and error handling |
| 4   | Test API endpoints using the REST Client extension              |
| 5   | Deploy a REST API to Azure App Service                          |

---

## 3. Requirements

This assignment consists of five parts.

### 3.1 Part 1: Add a Tasks Resource

Extend the Flask API to include a new `tasks` resource with the following endpoints:

| Endpoint      | Method | Description                  | Success Code |
| ------------- | ------ | ---------------------------- | ------------ |
| `/tasks`      | GET    | Retrieve all tasks           | 200          |
| `/tasks/<id>` | GET    | Retrieve a single task by ID | 200          |
| `/tasks`      | POST   | Create a new task            | 201          |
| `/tasks/<id>` | PUT    | Update an existing task      | 200          |
| `/tasks/<id>` | DELETE | Delete a task                | 204          |

#### 3.1.1 Task Data Structure

Each task must contain the following fields:

```json
{
  "id": 1,
  "title": "Complete Assignment 1",
  "description": "Extend the REST API with tasks",
  "user_id": 1,
  "completed": false
}
```

**Field Specifications:**

| Field         | Type    | Required | Default | Description                        |
| ------------- | ------- | -------- | ------- | ---------------------------------- |
| `id`          | integer | Auto     | —       | Unique identifier (auto-generated) |
| `title`       | string  | Yes      | —       | Task title                         |
| `description` | string  | No       | `""`    | Task description                   |
| `user_id`     | integer | Yes      | —       | Must reference an existing user    |
| `completed`   | boolean | No       | `false` | Task completion status             |

#### 3.1.2 Validation Requirements

Your API must handle the following error cases with appropriate HTTP status codes:

| Scenario                                      | Response Code   |
| --------------------------------------------- | --------------- |
| Task not found                                | 404 Not Found   |
| Missing required field (`title` or `user_id`) | 400 Bad Request |
| Invalid `user_id` (user doesn't exist)        | 400 Bad Request |
| Invalid JSON in request body                  | 400 Bad Request |

#### 3.1.3 Initial Data

Include at least two sample tasks in your in-memory data store:

```python
tasks = [
    {"id": 1, "title": "Learn REST", "description": "Study REST principles", "user_id": 1, "completed": True},
    {"id": 2, "title": "Build API", "description": "Complete the assignment", "user_id": 2, "completed": False},
]
```

---

### 3.2 Part 2: Add a User-Tasks Endpoint

Add an endpoint to retrieve all tasks assigned to a specific user:

| Endpoint            | Method | Description                            | Success Code |
| ------------------- | ------ | -------------------------------------- | ------------ |
| `/users/<id>/tasks` | GET    | Retrieve all tasks for a specific user | 200          |

**Behavior:**
- Return `404 Not Found` if the user does not exist
- Return an empty list `[]` if the user exists but has no tasks

---

### 3.3 Part 3: API Testing File

Create a file named `test-tasks-api.http` that tests all your new endpoints.

Your testing file must include the following nine test cases:

| #   | Test Case                        | Expected Result       |
| --- | -------------------------------- | --------------------- |
| 1   | GET all tasks                    | 200 with tasks array  |
| 2   | GET a single task                | 200 with task object  |
| 3   | GET a task that doesn't exist    | 404                   |
| 4   | POST a new task (valid data)     | 201 with created task |
| 5   | POST a task with missing title   | 400                   |
| 6   | POST a task with invalid user_id | 400                   |
| 7   | PUT to update a task             | 200 with updated task |
| 8   | DELETE a task                    | 204                   |
| 9   | GET tasks for a specific user    | 200 with user's tasks |

Refer to `test-api.http` in the Week 2 lab repository for file format examples.

---

### 3.4 Part 4: Deploy to Azure App Service

Deploy your extended API to Azure App Service so it is publicly accessible. Refer to the Week 2 lab repository for deployment instructions.

> **Important:** Delete all Azure resources immediately after recording your video demo to avoid unexpected charges. Your video serves as proof that the deployment worked.

---

### 3.5 Part 5: Video Demo

Create a short video demonstration showing your API running on Azure.

#### 3.5.1 Video Requirements

| Requirement | Details                                        |
| ----------- | ---------------------------------------------- |
| **Length**  | Maximum 5 minutes                              |
| **Audio**   | Not required (silent is acceptable)            |
| **Upload**  | YouTube (unlisted)                             |
| **Content** | Screen recording showing your API being tested |

#### 3.5.2 Video Content

Your video must demonstrate the following:

| Step | Description                                                      |
| ---- | ---------------------------------------------------------------- |
| 1    | Open your Azure URL in a browser to show the welcome message     |
| 2    | Run your `test-tasks-api.http` file against your Azure URL       |
| 3    | Show at least one error case (e.g., POST with invalid `user_id`) |

> **Tip:** Before recording, update your `test-tasks-api.http` to use your Azure URL:
> ```http
> ### 1. Get all tasks
> GET https://your-app-name.azurewebsites.net/tasks
> ```

#### 3.5.3 Recording Tools

- [OBS Studio](https://obsproject.com/) (free, cross-platform)

#### 3.5.4 Uploading to YouTube

1. Go to [YouTube Studio](https://studio.youtube.com/)
2. Click **Create** → **Upload video**
3. Set visibility to **Unlisted** (only people with the link can view)
4. Copy the video URL for your Brightspace submission

---

## 4. Submission Instructions

### 4.1 Preparation Steps

1. **Fork** the Week 2 lab repository (if you haven't already)
2. Add your changes to `app.py`
3. Create the `test-tasks-api.http` file
4. **Push** your code to your GitHub repository
5. **Deploy** your app to Azure App Service
6. **Record** and upload your video to YouTube (unlisted)

### 4.2 Brightspace Submission

Submit your **GitHub Repository URL** on Brightspace.

**Repository Requirements:**
- Repository must be **private**
- Add `ramymohamed10` as a collaborator (Settings → Collaborators → Add people)
- Include the YouTube video link in your `README.md` file

> **Warning:** It is your responsibility to ensure the instructor is added as a collaborator before the deadline. Submissions without collaborator access at the time of marking will not be accepted.

---

## 5. Marking Rubric

| Part | Component            |  Weight  |
| ---- | -------------------- | :------: |
| 1    | Tasks CRUD Endpoints |   25%    |
| 2    | User-Tasks Endpoint  |   10%    |
| 3    | API Testing File     |   10%    |
| 4    | Azure Deployment     |   15%    |
| 5    | Video Demo           |   40%    |
|      | **Total**            | **100%** |

---

## 6. Academic Integrity

This assignment **permits** and **encourages** the use of generative AI tools as per the course outline. I suggest using AI in **agent mode** (e.g., Claude Code, GitHub Copilot Agent) to assist with development.

**If you use AI assistance, you must:**

1. Disclose its use in a comment at the top of your code
2. Understand and be able to explain all code you submit
3. Ensure the code works correctly — AI-generated code with bugs will lose marks

**Example disclosure:**
```python
# AI Disclosure: Claude Code was used in agent mode to assist with
# implementing the tasks CRUD endpoints.
```

For complete details, refer to the **Course Policy on Generative AI Use** in the course outline.

---

## Questions?

Email: mohamer@algonquincollege.com

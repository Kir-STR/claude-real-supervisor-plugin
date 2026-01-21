# Product Requirements Document: Task Management API

## Overview

We need to build a RESTful API for a task management system that allows users to create, read, update, and delete tasks. The API should support task categorization, priority levels, and due dates.

## Objectives

1. Provide a simple, intuitive API for task management
2. Support multiple users with authentication
3. Enable task organization through categories and priorities
4. Allow filtering and searching of tasks

## Functional Requirements

### FR-1: Task CRUD Operations
- Users must be able to create new tasks with title, description, category, priority, and due date
- Users must be able to read their own tasks (individual and list views)
- Users must be able to update task properties
- Users must be able to delete tasks they own

### FR-2: Task Properties
- **Title**: Required, 1-200 characters
- **Description**: Optional, up to 2000 characters
- **Category**: Optional, predefined set (Work, Personal, Shopping, Health, Other)
- **Priority**: Required, enum (Low, Medium, High, Urgent)
- **Due Date**: Optional, ISO 8601 format
- **Status**: Required, enum (Todo, In Progress, Completed, Archived)
- **Created At**: Auto-generated timestamp
- **Updated At**: Auto-updated timestamp

### FR-3: User Authentication
- Users must authenticate using JWT tokens
- API must support user registration and login
- Each user can only access their own tasks

### FR-4: Task Filtering & Search
- Filter tasks by: category, priority, status, date range
- Search tasks by: title and description text match
- Support pagination for list endpoints (page size: 20 items)

### FR-5: Task Statistics
- Provide endpoint for task statistics: count by status, count by priority, upcoming due dates

## Non-Functional Requirements

### NFR-1: Performance
- API response time: < 200ms for single task operations
- API response time: < 500ms for list operations
- Support at least 100 concurrent users

### NFR-2: Security
- All endpoints except registration/login require authentication
- Passwords must be hashed using bcrypt
- JWT tokens expire after 24 hours
- Input validation on all endpoints to prevent injection attacks

### NFR-3: Data Persistence
- Use a relational database (PostgreSQL preferred)
- Database schema must support future extensions
- Implement proper indexing for query performance

### NFR-4: API Design
- Follow RESTful principles
- Use standard HTTP status codes
- Return JSON responses with consistent structure
- Include proper error messages with error codes

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login and receive JWT token

### Tasks
- `POST /api/tasks` - Create new task
- `GET /api/tasks` - List tasks (with filtering and pagination)
- `GET /api/tasks/:id` - Get single task
- `PUT /api/tasks/:id` - Update task
- `DELETE /api/tasks/:id` - Delete task
- `GET /api/tasks/stats` - Get task statistics

## Data Models

### User
```
{
  id: UUID,
  email: string (unique),
  password: string (hashed),
  name: string,
  created_at: timestamp
}
```

### Task
```
{
  id: UUID,
  user_id: UUID (foreign key),
  title: string,
  description: string,
  category: enum,
  priority: enum,
  status: enum,
  due_date: timestamp (nullable),
  created_at: timestamp,
  updated_at: timestamp
}
```

## Constraints

- Must be implemented in Node.js with Express framework
- Must use PostgreSQL database
- Must include input validation using a validation library
- Must follow MVC architecture pattern
- Code must include basic error handling

## Out of Scope

The following are explicitly NOT required for this version:
- User profile management beyond registration
- Task sharing or collaboration features
- Email notifications
- Real-time updates via WebSockets
- Mobile app development
- Automated testing suite (basic manual testing is sufficient)
- Deployment configuration or CI/CD pipeline

## Success Criteria

The API will be considered successful when:
1. All functional requirements are implemented and working
2. Authentication is secure and functional
3. API endpoints return correct data with proper status codes
4. Database schema is created with appropriate relationships
5. Basic manual testing shows all features working correctly

## Deliverable

A complete Node.js/Express application with:
- Source code organized in MVC structure
- Database schema and models
- API endpoints implementation
- Basic documentation of endpoints
- Instructions for local setup and running

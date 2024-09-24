# ProjectManager

Project Manager is a microservices-based application for managing projects and users. It consists of three main services: ProjectManagementService, UserManagementService, and NotificationService.

## Architecture Overview

The application is built using a microservices architecture, with each service having its own responsibility and database. The services are developed using .NET 8.0 and use MongoDB as their database.

### Services

1. **ProjectManagementService**: Handles project-related operations.
2. **UserManagementService**: Manages user accounts and authentication.
3. **NotificationService(TODO)**: Handles sending and processing of notifications.

All services are structured as separate .NET projects and are included in the main repository as Git submodules.

## ProjectManagementService

This service is responsible for managing projects and tasks within projects.

### Key Components:

- **Models**: 
  - Project
  - ProjectTask
  - UserInfo

- **Controllers**: 
  - ProjectController

- **Services**: 
  - ProjectService
  - SqsService

- **DTOs**: 
  - Various request and response DTOs for API operations

### Main Functionalities:

- CRUD operations for projects
- Adding and removing project members
- Creating, updating, and deleting tasks within projects
- Sending notifications to users via AWS SQS
## UserManagementService

This service handles user management and authentication.

### Key Components:

- **Models**: 
  - User

- **Controllers**: 
  - UserController

- **Services**: 
  - UserService
  - PasswordHasherService

- **DTOs**: 
  - Various request and response DTOs for API operations

### Main Functionalities:

- User registration and login
- JWT-based authentication
- User profile management
- Password change
- User search

## NotificationService

This service is responsible for handling and sending notifications.

### Main Functionalities:

- Polling notification messages from AWS SQS
- Processing different types of notifications
- Sending notifications to users (e.g., via email or push notifications)

### Workflow:

1. ProjectManagementService sends notification messages to AWS SQS.
2. NotificationService polls messages from the SQS queue.
3. NotificationService processes the messages and sends appropriate notifications.

## Message Queue

The project uses AWS Simple Queue Service (SQS) for asynchronous communication between ProjectManagementService and NotificationService.

## Authentication

Both services use JWT (JSON Web Tokens) for authentication. The UserManagementService is responsible for issuing tokens, while the ProjectManagementService validates these tokens for protected endpoints.

## Database

Each service uses its own MongoDB database. The connection strings and database names are stored securely using AWS Parameter Store.

## Configuration and Secrets Management

Sensitive configuration data such as database connection strings and JWT secret keys are managed using AWS Parameter Store. This approach enhances security by keeping sensitive information out of the codebase.

## API Documentation

Both services use Swagger for API documentation. When running in development mode, you can access the Swagger UI at `/swagger` endpoint for each service.

## Getting Started

1. Clone the repository with submodules:
   ```
   git clone --recurse-submodules https://github.com/rrroy5640/ProjectManager.git
   ```

2. Set up MongoDB databases for all three services.

3. Configure AWS Parameter Store with necessary secrets.

4. Set up AWS SQS queue for notifications.

5. Run each service separately:
   ```
   cd services/ProjectManagementService
   dotnet run

   cd services/UserManagementService
   dotnet run

   cd services/NotificationService
   dotnet run
   ```

6. Access the Swagger UI for API documentation and testing:
   - ProjectManagementService: http://localhost:5272/swagger
   - UserManagementService: http://localhost:5069/swagger
   - NotificationService: http://localhost:5xxx/swagger (replace with actual port)

## Future Improvements

- Implement API Gateway for routing requests to appropriate services
- Add more comprehensive logging and monitoring
- Implement caching for frequently accessed data
- Containerize services using Docker for easier deployment and scaling
- Add more notification channels for NotificationService (e.g., SMS, mobile push)
- Implement frontend with React.js

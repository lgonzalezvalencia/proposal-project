# Lightweight Proposal Generator

## Core Features for MVP

### Template Management:

Upload or create proposal templates with placeholders (e.g., {{ClientName}}, {{Price}}).
Proposal Editor:
Simple UI to fill in key details for each proposal.

### PDF Export:

Generate a downloadable PDF of the completed proposal.
User Authentication:
Basic login/signup for sales teams.

### PDF Generation:

Use libraries like pdf-lib (JS), WeasyPrint (Python), or Puppeteer (for HTML-to-PDF).

## Microservices Architecture:

- Database backend - this will save template names, content, and user data.
- PDF Generation and storage - generate PDFs from template content (retrieved from kafka), store them, and send back a URL to the user and into the database.
- Frontend - this will allow users to select templates, fill in details, and download the generated PDF.
- Authentication - basic user management to ensure only authorized users can create and download proposals. It has its own database table for user credentials.
- kafka - used for message passing between the frontend and backend, especially for generating PDFs asynchronously.
- template management - allows users to create, edit, and delete proposal templates.

  All this managed by docker-compose for easy deployment and management.

### Database Backend

Purpose: Stores templates, proposals, user data.
Comment: Essential. Use a relational DB (PostgreSQL) for structured data, or MongoDB if you want more flexibility.

### PDF Generation & Storage Microservice

Purpose: Receives template data, generates PDF, stores it, returns URL.
Comment:
Using a microservice for this is great for scalability.
For MVP, you might skip storage and just return the PDF directly, then add storage (S3, local, etc.) later.

### Frontend

Purpose: User interface for template selection, editing, proposal creation, and download.
Comment:
React or Vue are good choices.
Keep it simple for MVP—focus on usability.

### Authentication Service

Purpose: Handles user registration, login, and session management.
Comment:
For MVP, consider using Firebase Auth or Auth0 to save time, unless you specifically want to build your own.

### Kafka for Message Passing

Purpose: Asynchronous communication, especially for PDF generation.
Comment:
Kafka is powerful, but may be overkill for an MVP unless you expect heavy load or need strict decoupling.
For a small team and MVP, a simple REST API call (synchronous) may be faster to implement and easier to debug.
You can refactor to Kafka or another message queue (e.g., RabbitMQ) as you scale.

### Template Management Microservice

Purpose: CRUD operations for templates.
Comment:
Good separation of concerns.
For MVP, this can be part of the main backend and split out later.

### Docker Compose for Deployment

Purpose: Orchestrates all services for local development and deployment.
Comment:
Excellent choice for managing dependencies and environments.

## Recommendations for MVP

### Start Simple:

For MVP, minimize moving parts. Consider starting with:
A single backend service (handles templates, proposals, PDF generation, and auth).

- Frontend.
- Database.
- Add microservices and Kafka as you scale or need async processing.

## PDF Generation:

- If using Node.js, pdf-lib or Puppeteer (HTML-to-PDF) are both good.
- If using Python, WeasyPrint or ReportLab.

## Authentication:

- Use a managed service if speed is critical.

## Asynchronous Processing:

- Only add Kafka if you have a clear need for async or heavy workloads.

| Component      | MVP Approach      | Scale-Up Approach         |
| -------------- | ----------------- | ------------------------- |
| Backend        | Monolith          | Microservices             |
| PDF Generation | Inline in backend | Dedicated microservice    |
| Auth           | Firebase/Auth0    | Custom microservice       |
| Messaging      | REST (sync)       | Kafka/RabbitMQ (async)    |
| Deployment     | Docker Compose    | Docker Compose/Kubernetes |

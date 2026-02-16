# Express.js CI/CD Learning App

A simple Express.js application to learn CI/CD integration using Jenkins and GitHub.

## APIs

### 1. Health Check
```bash
GET http://localhost:3000/api/health
```

Response:
```json
{
  "status": "OK",
  "message": "Server is running",
  "timestamp": "2024-01-01T00:00:00.000Z"
}
```

### 2. Get All Users
```bash
GET http://localhost:3000/api/users
```

Response:
```json
{
  "success": true,
  "count": 3,
  "data": [
    { "id": 1, "name": "John Doe", "email": "john@example.com" },
    { "id": 2, "name": "Jane Smith", "email": "jane@example.com" },
    { "id": 3, "name": "Bob Johnson", "email": "bob@example.com" }
  ]
}
```

### 3. Add New User
```bash
POST http://localhost:3000/api/users
Content-Type: application/json

{
  "name": "New User",
  "email": "newuser@example.com"
}
```

## Setup Instructions

### Local Setup
```bash
# Install dependencies
npm install

# Start the server
npm start
```

### Jenkins CI/CD Setup

1. **Install Jenkins** on your machine or server
2. **Install Node.js** plugin in Jenkins
3. **Create a new Pipeline job** in Jenkins
4. **Configure GitHub** as the source code repository
5. **Point to the Jenkinsfile** in the repository
6. **Run the pipeline**

## Pipeline Stages

The Jenkins pipeline includes:
- **Checkout**: Pulls code from GitHub
- **Install Dependencies**: Runs `npm install`
- **Lint**: Code quality checks
- **Test**: Runs tests
- **Build**: Builds the application
- **Deploy**: Deploys the application

## GitHub Webhook Setup

To trigger Jenkins builds on code push:

1. Go to your GitHub repository
2. Navigate to Settings > Webhooks
3. Add a new webhook:
   - Payload URL: `http://<your-jenkins-url>/github-webhook/`
   - Content type: `application/json`
   - Events: Push events


# KTS Backend System

## Description
This is the core backend API for the KTS platform, built with Node.js, Express, and MongoDB. It handles data processing, authentication, and business logic for both the admin and user frontends.

## 1. How to Set Up Envs
Create a `.env` file in the root directory. You will typically need to configure:
- `PORT`: The server port (e.g., `4000`)
- `MONGO_URI`: The MongoDB connection string
- `JWT_SECRET`: Secret key for JWT authentication
- `CLOUDINARY_URL`: Cloudinary connection URL for media storage
- `EMAIL_HOST`: SMTP host for Nodemailer (e.g., `smtp-mail.outlook.com`)
- `EMAIL_USER`: Email address for SMTP authentication
- `EMAIL_PASS`: Password or app password for SMTP authentication
- `NODE_ENV`: Node environment (e.g., `development`, `production`)

## 2. How to Run in Dev Env
To run the server in a development environment:
1. Make sure you have Node.js and npm installed.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the server with hot-reload (nodemon):
   ```bash
   npm run dev
   ```
   Alternatively, you can use the Docker development environment:
   ```bash
   docker-compose -f compose.dev.yaml up --build
   ```

## 3. How to Run in Prod
For production, you can start the Node.js server using:
```bash
npm start
```
Or use the Docker production setup:
```bash
docker-compose -f compose.prod.yaml up -d --build
```

## 4. How to Use infra Repo to Deploy in AWS and Create a Pipeline
AWS deployment and infrastructure provisioning are governed by the **KTS-infra** repository.
1. Use the `KTS-infra` repository to deploy the required AWS infrastructure, including databases, networking, and compute environments (EKS/ECS).
2. Set up the CI/CD pipeline using the provided `Jenkinsfile`.
3. The pipeline automation will:
   - Run tests and static analysis with SonarQube (`sonar-project.properties`).
   - Build the backend Docker image using `Dockerfile.prod`.
   - Push the resulting image to the AWS ECR registry.
4. Finally, your deployment orchestrator (e.g., Argo CD, AWS CodeDeploy) will update the application running in AWS to use the newly pushed image.

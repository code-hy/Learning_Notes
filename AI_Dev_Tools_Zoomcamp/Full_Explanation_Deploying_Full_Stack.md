# Comprehensive Guide: Deploying Full-Stack Applications

This guide provides a detailed, beginner-friendly explanation of how to containerize, test, and deploy a full-stack web application to production [2, 3]. It covers the complete lifecycle of moving an application from local development to cloud deployment on Amazon Web Services (AWS) using modern containerization, database management, automated testing, and CI/CD pipelines [3, 17].

---

## 1. Overview & Architecture Transition

Understanding the difference between how an application runs during **development** and how it operates in **production** is fundamental to full-stack deployment [5, 6].

### Development vs. Production Setup
* **Development Environment**: During local development, the frontend and backend typically run as two separate services in two separate terminals [5].
  * The frontend uses a development server (such as Vite) that watches code changes and updates the browser instantly [5].
  * The backend runs on a separate local port and handles database queries [4, 5].
* **Production Environment**: In production, the frontend code does not change while running [6]. The React frontend is compiled once into static HTML, CSS, and JavaScript files [4, 6].
  * Instead of running two separate containers, the backend web server (such as FastAPI) serves these compiled static frontend files directly [6].
  * This allows the entire application to run efficiently within a **single container** [6, 17].

---

## 2. Containerization with Docker

Containerization packages an application and all its dependencies into a standard unit called a container [6, 7].

### Multi-Stage Docker Builds
To create a single lightweight production container, a **two-stage Docker build** is used [7]:
1. **Stage 1 (Frontend Build)**: Uses a Node.js Docker image to compile the React frontend into static assets [7].
2. **Stage 2 (Backend Packaging)**: Uses a Python Docker image for the backend and copies only the compiled static files from Stage 1, leaving heavy Node.js development dependencies behind [7].

### Docker Commands & Volumes
* **Building the Docker Image**:
  ```bash
  docker build -t sdip:latest .
  ```
  This command compiles the frontend and packages the backend into a container image tagged `sdip:latest` [7].

* **Running the Container with Persistent Storage**:
  ```bash
  docker run --rm -p 8000:8000 \
    -v sdip-data:/data \
    -e SDIP_DATABASE_URL=sqlite:////data/sdip.db \
    --name sdip sdip:latest
  ```
  The `-v sdip-data:/data` flag mounts a named Docker volume, ensuring that database files (such as SQLite) persist between container restarts [7].

---

## 3. Database Migration: Switching from SQLite to Postgres

Databases store application data, but different databases serve different purposes depending on the environment [8].

### SQLite vs. PostgreSQL
* **SQLite**: Keeps all data in a single local file and does not require a database server [8]. It is extremely convenient for local development [8].
* **PostgreSQL (Postgres)**: A robust database server designed for production environments where higher concurrency and reliability are required [8].

### Using Object-Relational Mapping (ORM)
By using an ORM like SQLAlchemy in the backend code, the application abstracts database operations [4, 9]. This allows the database backend to be switched from SQLite to Postgres easily by updating the database connection URL [8, 9, 10].

* **Starting Postgres via Docker**:
  ```bash
  docker run -d \
    --name interview-canvas-db \
    -e POSTGRES_USER=sdip \
    -e POSTGRES_PASSWORD=sdip \
    -e POSTGRES_DB=sdip \
    -p 5432:5432 \
    -v interview-canvas-pgdata:/var/lib/postgresql/data \
    postgres:16-alpine
  ```
  This command starts a local Postgres database container with persistent data storage [9].

* **Connecting Backend to Postgres**:
  ```bash
  export SDIP_DATABASE_URL=postgresql://sdip:sdip@localhost:5432/sdip
  make run
  ```
  Setting the `SDIP_DATABASE_URL` environment variable directs the backend to store data in Postgres [10].

---

## 4. Multi-Service Management with Docker Compose

When an application requires multiple services (such as an application container and a database container), starting them with individual commands becomes tedious [10].

### Docker Compose
Docker Compose allows developers to define and run multi-container applications using a single configuration file (`docker-compose.yaml`) [10].

### Key Features of Docker Compose Setup
* **Service Coordination**: Combines the application and Postgres database services into one unified setup [10].
* **Health Checks**: Ensures the application container waits until the Postgres database is healthy and ready to accept network connections before attempting to start [10].
* **Single-Command Execution**:
  ```bash
  docker compose up --build
  ```
  This command builds all images and starts the entire multi-container stack simultaneously [10, 11].

---

## 5. Automated & End-to-End Testing

Automated testing verifies that application components function correctly together after system changes [11].

### Types of Tests
1. **Integration Tests**: Verify that the compiled frontend static files load correctly and that the backend can successfully communicate with the Postgres database [11].
2. **End-to-End (E2E) Tests**: Simulate real user interactions across the entire system using testing tools like Playwright [12].

### Playwright Automated E2E Scenario
The automated E2E test suite simulates a real-time collaborative session [4, 12]:
1. **Interviewer Session**: Logs in as an interviewer and creates an interview canvas session [12].
2. **Share Link**: Generates and shares the unique session join link [12].
3. **Candidate Session**: Opens a second browser client as the candidate joining via the link [12].
4. **Canvas Update**: Candidate modifies an element on the canvas [12].
5. **Real-Time Verification**: Asserts that the interviewer's session receives and displays the canvas update in real time [12].

* **Running E2E Tests**:
  ```bash
  make e2e
  ```
  Executes the full Playwright suite against the running Docker Compose environment [12].

---

## 6. Deploying to Amazon Web Services (AWS)

Once local testing passes, the application is ready for cloud deployment [13].

### Deployment Options
While managed container hosting services like Render, Railway, or Fly.io offer quick deployment, cloud providers like AWS offer full control via infrastructure-as-code tools like AWS CloudFormation [13, 14].

### AWS CloudFormation Single-Instance Architecture
A single-instance proof-of-concept deployment on AWS EC2 uses CloudFormation (`sdip-stack.yaml`) [14]:
* **Caddy Web Server**: Acts as a reverse proxy, handling HTTPS security certificates and WebSocket (WSS) connections for real-time canvas updates [14, 18].
* **Application Container**: Runs the FastAPI backend serving static frontend assets [6, 14].
* **Postgres Container**: Runs the database on the same instance [14].

*(Note: For production environments, using dedicated managed database services like AWS RDS is recommended for greater reliability and scalability [14].)*

---

## 7. CI/CD Pipelines with GitHub Actions

Continuous Integration and Continuous Deployment (CI/CD) automate testing and deployment processes whenever code changes are committed [15].

### Core Concepts
* **Continuous Integration (CI)**: Automatically runs test suites every time code is pushed to the repository to catch errors early [15].
* **Continuous Deployment (CD)**: Automatically deploys code updates to the cloud environment once all automated tests pass [15].

### GitHub Actions Workflow Execution
The automated pipeline executes the following steps upon pushing to the `main` branch [15, 16]:
1. **Parallel Unit Testing**: Executes backend and frontend unit test suites in parallel [15, 16].
2. **Stack Build & Test**: Builds the Docker Compose stack and executes integration and Playwright E2E tests against it [15, 16].
3. **AWS Authentication via OIDC**: Uses OpenID Connect (OIDC) roles to authenticate securely with AWS without storing permanent access keys in GitHub [15, 16].
4. **Deployment**: Updates the AWS infrastructure stack with the new application version [15, 16].
5. **Health Validation**: Verifies deployment success by checking the application's health endpoint [16].

---

## 8. Resource Cleanup & Next Steps

### Cleaning Up Cloud Resources
When cloud environments are no longer needed, deleting the CloudFormation stack removes all associated AWS resources to prevent unnecessary costs [17]:
```bash
aws cloudformation delete-stack --stack-name sdip
aws cloudformation wait stack-delete-complete --stack-name sdip
```

### Future Production Considerations
For long-term production operations, additional DevOps practices should be implemented [18]:
* Separating development and production environments [18].
* Setting up observability tools for logging, performance metrics, and automated alerts [18].
* Implementing automated observe-and-respond loops for incident detection and remediation [18].

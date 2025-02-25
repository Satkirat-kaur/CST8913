# 1. Overview

## Objective

Modernize a legacy face recognition application—from a monolithic system running on a Windows Server 2019 VM—to a cloud-native solution on Microsoft Azure. In addition to refactoring the application into microservices, the new design integrates Azure Cognitive Services (Face API) to enhance facial recognition features while improving scalability, resilience, and cost efficiency.

### Scenario
A company uses a face recognition app on a Windows Server 2019 VM in Azure. The app includes:
- A web frontend for user interaction.
- Business logic that processes face data.
- A SQL Server database for customer and image data.
- In-house tasks for image processing and face matching.

Now, the company wants to break the app into smaller, scalable services using Azure.

## 2. Assessing the Existing Architecture

### Monolithic Components
- **Frontend/UI:** A single interface for uploading images and viewing results.
- **Face Recognition & Business Logic:** Combined modules that handle image processing, face detection, and matching.
- **Data Access:** Direct SQL Server connections for customer data, face details, and logs.
- **Background Processing:** Batch jobs for image processing, logging, and face matching.
- **Static Content:** Local file storage for images, logs, and other assets.

![Alt text](https://github.com/Satkirat-kaur/CST8913/blob/main/Lab%206/Monolithic%20Architecture.png)

## 3. Planning the Refactoring Strategy

### Breaking Down the Monolithic Application

1. **Frontend Migration**
   - **Task:** Separate the web UI.
   - **Service:** Use Azure App Service for better scaling and managed hosting.

2. **Business Logic Separation**
   - **Task:** Extract face recognition and other logic into separate APIs.
   - **Service:** Deploy as microservices on Azure App Service or use containerized services on Azure Kubernetes Service (AKS).

3. **Enhancing Facial Recognition**
   - **Task:** Offload heavy face detection and recognition.
   - **Service:** Integrate Azure Cognitive Services Face API.

4. **Database Migration**
   - **Task:** Move from a VM-hosted SQL Server.
   - **Service:** Use Azure SQL Database for improved performance and security.

5. **Background Processing**
   - **Task:** Change scheduled image processing and matching tasks.
   - **Service:** Use Azure Functions for serverless and event-driven processing.

6. **Static Content and Logging**
   - **Task:** Move images, static files, and logs.
   - **Service:** Use Azure Blob Storage.

7. **Authentication & Authorization**
   - **Task:** Secure access to services and APIs.
   - **Service:** Use Azure Active Directory (Azure AD) for user authentication.

**Architecture Diagram**
![Alt text](https://github.com/Satkirat-kaur/CST8913/blob/main/Lab%206/Cloud-Native%20Architecture.png)

## 4. Implementation Steps

### A. Deploying the Frontend to Azure App Service
- **Code Refactoring:** Make sure the UI is separate from the backend logic and is stateless.
- **Deployment:** Use CI/CD pipelines (like Azure DevOps or GitHub Actions) or deploy via the Azure portal.
- **Configuration:** Set up connection strings and environment variables for the new services and Face API.

### B. Migrating the Database to Azure SQL Database
- **Preparation:** Review and document the current SQL Server schema.
- **Migration:** Use tools like the Data Migration Assistant or Azure Database Migration Service.
- **Validation:** Test data integrity and performance after migration.

### C. Converting Background Tasks to Azure Functions
- **Task Identification:** List all image processing, log aggregation, and face matching tasks.
- **Development:** Create functions in C#, Python, or JavaScript with triggers (HTTP, Timer, or Queue).
- **Deployment:** Publish the functions using the Azure portal or CI/CD pipelines.
- **Integration:** Ensure functions communicate with the database and Blob Storage.

### D. Integrating Azure Cognitive Services Face API
- **Assessment:** Identify parts of the face recognition logic to move to the cloud.
- **Integration:**
  - Sign up for the Face API in Azure.
  - Update the app to send image data to the Face API.
  - Process the responses and integrate the results.
- **Benefits:** Use Microsoft’s AI models to improve accuracy and reduce maintenance.

### E. Moving Static Content & Logs to Azure Blob Storage
- **Static Assets:** Upload images, CSS, JavaScript, and other files to Blob Storage.
- **Log Storage:** Configure the app to write logs to Blob Storage or use Application Insights.

### F. Implementing Authentication & Authorization
- **Azure AD Setup:** Configure Azure AD to manage user identities.
- **Integration:** Use OAuth or OpenID Connect to secure endpoints in your app and APIs.

## 5. Optimization and Testing

### Auto-Scaling and Resilience
- **App Service Scaling:** Set auto-scaling rules based on traffic.
- **Azure Functions:** Use the consumption plan to automatically scale functions.
- **Resilience:** Use retry policies and error handling in all services.

### Logging and Monitoring
- **Application Insights:** Enable Application Insights for the frontend, APIs, and functions.
- **Azure Monitor:** Set up alerts and dashboards to monitor resource use and service health.

### Performance and Integration Testing
- **Load Testing:** Use Azure Load Testing or tools like Apache JMeter to simulate high traffic.
- **Integration Testing:** Test that the frontend, APIs, Face API, database, and storage work well together.
- **Failure Testing:** Simulate outages (e.g., loss of connectivity to the Face API) to test fallback mechanisms.

## 6. Service Justifications
- **Azure App Service:** Offers scalable and managed hosting for the frontend and APIs.
- **Azure SQL Database:** Provides reliable and secure data storage.
- **Azure Functions:** Supports cost-effective, event-driven background processing.
- **Azure Blob Storage:** Gives scalable storage for static content and logs.
- **Azure Cognitive Services (Face API):** Improves face recognition using advanced AI.
- **Azure AD:** Ensures secure, centralized authentication and authorization.

## 7. Migration & Deployment Steps
1. **Frontend:** Refactor the UI and deploy it to Azure App Service.
2. **Database:** Migrate from a VM-hosted SQL Server to Azure SQL Database.
3. **Background Tasks:** Convert these tasks into Azure Functions.
4. **Face Recognition:** Integrate the Face API into the recognition workflow.
5. **Static Assets & Logs:** Move them to Azure Blob Storage.
6. **Monitoring & Scaling:** Set up auto-scaling, Application Insights, and Azure Monitor.

## 8. Lessons Learned & Challenges
- Separating parts of the old face recognition system took careful planning.
- Using a third-party AI service (Face API) improved accuracy and reduced maintenance.
- Ensuring data consistency during migration was challenging but succeeded with proper testing.

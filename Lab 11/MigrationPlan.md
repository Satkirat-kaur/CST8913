# Migration Plan – Tailwind OpenCare

This document outlines the step-by-step strategy to migrate the **Patient Self-Booking System** from Tailwind OpenCare’s on-premises data center to **Microsoft Azure**. The goal is to reduce operational overhead, improve scalability, meet compliance needs, and ensure secure, cost-effective migration — all within a **downtime window of less than 2 hours**.

---

## 1. Discovery and Tooling Setup

Before we begin the actual migration, we need to understand the current infrastructure.

### Tools to Use:
- **Azure Migrate: Discovery and Assessment Tool**
  - Recommended to use the **appliance-based** option for more detailed data.

### Information to Collect:
- Operating system details (Ubuntu versions)
- Installed software and services
- CPU, memory, and disk usage stats
- Network ports and services running
- Application dependencies and traffic patterns
- Storage usage and disk input/output (IOPS)

### Credential Management:
- Use **Azure Key Vault** to securely store all credentials used by Azure Migrate.
- Assign access based on **least privilege** using **Azure RBAC** (Role-Based Access Control).

---

## 2. Assessment Planning

Once we gather the system data, we need to assess how everything will fit into Azure.

### Key Assessment Parameters:
- Collect **at least 14 days** of performance data to capture usage trends.
- Use **performance-based sizing** to ensure right-sized resources.
- Include peak usage times for more accurate recommendations.

### Azure Configuration:
- **Region:** Canada Central (to maintain low latency and data residency)
- **Compute Type:** Azure Virtual Machines (B-series or D-series based on load)
- **Storage:**
  - Frontend/API servers: **Premium SSD**
  - Database: **Azure Database for PostgreSQL – Flexible Server**
  - Caching: **Azure Cache for Redis**
  - Backups: **Azure Blob Storage (Cool Tier)**

### Validating with Stakeholders:
- Share Azure Migrate’s report with IT, compliance, and app owners.
- Review estimated costs, performance, and high availability options.
- Confirm SLAs, recovery objectives, and regional placement.

---

## 3. Dependency Analysis and Optimization

Understanding how services interact is critical before migration.

### Dependency Mapping:
- Use **agentless dependency visualization** in Azure Migrate.
- For deeper insights (e.g., per-process data), deploy the **Dependency Agent**.

### Clean the Data:
- Remove noise by filtering out system-level connections like DNS or NTP.
- Focus on app-to-app communications (e.g., frontend-to-backend, backend-to-database).

### Metrics to Identify:
- **Criticality:** This is a core business system — high priority.
- **Users:** Directly patient-facing; must avoid long downtimes.
- **SLA Needs:** 99.9% uptime with daily incremental backups.
- **Backup:** Azure Backup with geo-redundant storage and 30-day retention.

---

## 4. Server Grouping and Migration Waves

Grouping servers allows us to migrate in a controlled way.

### Logical Grouping:
- **Group 1: Frontend** – 2 NGINX servers
- **Group 2: Backend** – 2 Node.js API servers
- **Group 3: Data Layer** – PostgreSQL + Redis
- **Group 4: Backup System** – Local backup service

### Migration Waves:
- **Wave 1:** Backup system (least impact)
- **Wave 2:** NGINX frontend servers (stateless, quick to test)
- **Wave 3:** Node.js API servers (test backend logic)
- **Wave 4:** Redis and PostgreSQL (critical data layer – migrated last)

### Migration Considerations:
- Set up **NSGs** and firewall rules ahead of time.
- Replace HAProxy with **Azure Load Balancer**.
- Ensure IP addresses (static if needed) are pre-assigned to avoid surprises.

---

## 5. Migration Plan Documentation 

Here is the actual step-by-step plan for moving everything to Azure.

### Step 1 – Preparation
- Set up the Azure Migrate appliance on-prem.
- Collect data and run assessments.
- Review findings with stakeholders.

### Step 2 – Pre-Migration
- Create Azure Resource Groups, VNets, and Subnets.
- Configure **Azure Load Balancer** with health probes.
- Set up **Azure Backup Vault** and define backup policies.
- Pre-configure DNS entries (internal and external as needed).

### Step 3 – Migration
- Use **Azure Migrate: Server Migration** tool to rehost:
  - NGINX and Node.js servers as Azure VMs.
  - PostgreSQL as **Azure Database for PostgreSQL**.
  - Redis as **Azure Cache for Redis**.
- Perform each wave during a planned **2-hour maintenance window**.

### Step 4 – Post-Migration
- Update all DNS records to new Azure IP addresses.
- Validate application functionality across all components.
- Run database integrity checks and API endpoint tests.
- Perform a test backup restore to ensure recoverability.

### Step 5 – Final Validation
- Application owners run full functionality tests.
- IT team monitors Azure metrics, performance, and backup jobs.
- Once confirmed, decommission the on-premises systems.

---



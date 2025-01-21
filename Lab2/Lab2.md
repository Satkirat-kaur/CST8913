# Deployment of Web Applications Using IaaS and PaaS

In this report, I will talk about how to deploy a web application consisting of three main components: a Flask web server, a React UI frontend, and a PostgreSQL database. I will focus on deploying this application using two cloud service models: **Infrastructure-as-a-Service (IaaS)** and **Platform-as-a-Service (PaaS)**.

## 1. Deployment Using IaaS (Infrastructure-as-a-Service)

In the IaaS model, we are responsible for managing the virtual machines (VMs) and the operating system (OS). We get more control over the setup, but we must manually configure everything. Here’s how we can deploy the web application using IaaS:

### Steps for Deployment:

#### 1. Web Server (Flask):
- Start by creating a Virtual Machine (VM) (Azure VM).
- Install the operating system (Linux)and the Flask web framework on this VM.
- Run the Flask application on this server.

#### 2. UI Frontend (React):
- We can either use the same VM as Flask or create a separate VM for the React app.
- We can choose separate VMs for Flask and React as they are ideal for larger, production-ready applications where performance, scalability, and flexibility are priorities. This setup provides more control and ensures better resource management for the consumer.
- Install the required React dependencies on the VM.

#### 3. Database (PostgreSQL):
- Create a separate VM for the Postgres database.
- Install PostgreSQL and configure it to store the app data.

#### 4. Networking:
- Set up a virtual network that allows the VM instances to communicate with each other.
- Configure public IPs to allow access to the Flask server and React app from the internet.

### IaaS Summary:
In this setup, we have full control over the infrastructure, but we must handle everything ourselves, including installing software and managing the network. The main advantage is flexibility, but the challenge is the complexity of management.

![alt text](https://github.com/Satkirat-kaur/CST8913/blob/main/Lab2/IAAS%202.png)

---

## 2. Deployment Using PaaS (Platform-as-a-Service)

In the PaaS model, the cloud provider manages most of the infrastructure for us, making it easier to focus on the application itself. We don’t need to worry about setting up the OS or servers. Instead, we can deploy the app using managed services that handle pretty much everything for us.

### Steps for Deployment:

#### 1. Web Server (Flask):
- Use a managed service like Azure App Service to deploy the Flask web server.
- Simply upload Flask code, and the service automatically handles the environment and scaling.

#### 2. UI Frontend (React):
- Use services like Azure Static Web Apps to deploy the React app.

#### 3. Database (PostgreSQL):
- For the database, use managed services like Azure Database for PostgreSQL.

#### 4. Networking:
- PaaS services take care of networking, including load balancing and scaling, so we don't have to worry about it.

### PaaS Summary:
In PaaS, the cloud provider manages most aspects of the infrastructure, allowing you to focus more on your application code. This simplifies the deployment process, reduces complexity, and provides automatic scaling. However, you have less control over the underlying infrastructure.

![alt text](https://github.com/Satkirat-kaur/CST8913/blob/main/Lab2/PAAS%20diagram.png)

---

## 4. Conclusion

Both IaaS and PaaS offer various advantages depending on our needs. IaaS gives us more control over the infrastructure, making it suitable for more customized setups, but it requires more effort to manage. PaaS, on the other hand, is easier to use as it reduces much of the complexity, handling scaling and infrastructure management for us. 




> When architecting web applications, I prioritize a consistent, **vendor-agnostic toolset** for my production environments. My primary goal is to minimize reliance on proprietary cloud services; I utilize cloud providers strictly for **VPS compute** to facilitate a self-hosted stack.
> 
> From the database to the networking layer, I traditionally **provision and configure** every component manually to maintain full architectural control. I am currently evolving this process by developing a custom **automation framework** to streamline these deployments while maintaining the same level of granularity. There’s something satisfying about being the cloud myself ☁️.
> 

## 🖥️ [Servers]

The backbone of my infrastructure sits on **DigitalOcean Droplets**. These managed VPS nodes provide the necessary high availability and predictable cost scaling for my current workloads.

## 🐋 [Containers Everywhere}

To manage these nodes, I implement either a **docker swarm** or **k3s cluster**. Practically, a docker swarm cluster suffices for most of my use cases, but occasionally, I run k3s for learning purposes and bettering my comfortability with kubernetes. I chose k3s specifically for its resource efficiency on my cheap low compute servers; it provides a full Kubernetes API without the memory overhead of a standard distribution. Every service is **containerized via Docker**, ensuring that the environment on my local workstation is an exact mirror of what runs in production. This eliminates the "works on my machine" class of deployment failures.

## 📬 [Message Broker]

To keep these services decoupled, I use **RabbitMQ** as my primary message broker. By moving toward an asynchronous, event-driven model, I ensure that messages and events that do not need to be handled immediately can be throttled, and in the event a spike in traffic occurs within one microservice, it will not lead to a cascading failure across the rest of the stack.

## 🗄️ [Databases]

My choice of database is based on the complexity of the expected workload. For high-integrity, relational data, I rely on **Microsoft SQL Server**. However, for lightweight, localized storage or edge-case services, I utilize **SQLite** to keep the architectural footprint small.

## 🌐 [Proxy]

Traffic enters the cluster through a **Caddy** proxy setup. Caddy operates as the primary entry point, handling **load balancing and traffic shaping** while providing fully automated TLS certificate management.

## 🤖 [CI/CD]

My current workflow is fully automated via **GitHub Actions**. Every `git push` triggers a pipeline that handles linting, unit testing, and image builds, pushing the final artifacts to my registry only when they pass all checks.

## 📈 [Logging & Monitoring]

I’ve standardized my logging and observability on **Grafana Loki**. Instead of SSHing into individual nodes to tail logs which was always a hassle, I have a centralized telemetry dashboard. This allows for proactive debugging—identifying performance bottlenecks before they become system-wide outages.

## ☁️ [Cloud-native Adventures]

While my architecture is focused on being provider-independent, I sometimes (rarely enough) leverage **AWS** to augment my infrastructure with managed, highly available services. My experience with the AWS ecosystem focuses on scalable messaging, container orchestration, and Infrastructure as Code (IaC).

- **Compute & Orchestration:** I have experience deploying and managing workloads using **EC2** for raw compute and **EKS (Elastic Kubernetes Service)** for managed orchestration. I utilize **ECR (Elastic Container Registry)** as a secure, private repository for my Docker artifacts with this setup.
- **Networking & Routing:** To manage traffic and high availability, I implement **ELB (Elastic Load Balancing)** to distribute incoming application requests across healthy targets.
- **Event-Driven Architecture:** I utilize **SNS**, **SQS**, and **EventBridge** to build resilient, decoupled systems, allowing for seamless communication between distributed microservices.
- **Provisioning:** To ensure repeatable and documented infrastructure, I use **Terraform** for cloud resource provisioning, treating my environment configurations with the same rigor as my application code.

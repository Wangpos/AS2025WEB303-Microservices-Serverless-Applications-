# **WEB303 Microservices & Serverless Applications**

## **Practical 4: Kubernetes Microservices with Kong Gateway**

### **Academic Report**

---

**Student Name:** Tshering Wangpo Dorji 
**Student ID:** 02230311
**Course:** WEB303 - Microservices & Serverless Applications  
**Semester:** Year 3, Semester I  

---

## **Table of Contents**

1. [Executive Summary](#1-executive-summary)
2. [Introduction](#2-introduction)
3. [Learning Objectives](#3-learning-objectives)
4. [System Architecture](#4-system-architecture)
5. [Implementation Process](#5-implementation-process)
6. [Technical Analysis](#6-technical-analysis)
7. [Challenges and Solutions](#7-challenges-and-solutions)
8. [Results and Testing](#8-results-and-testing)
9. [Critical Evaluation](#9-critical-evaluation)
10. [Conclusion](#10-conclusion)
11. [References](#11-references)
12. [Appendices](#12-appendices)

---

## **1. Executive Summary**

This report documents the successful implementation of a production-grade microservices application using Kubernetes orchestration, Kong API Gateway, and Consul service discovery. The project involved developing a Student Cafe ordering system comprising three main components: a React.js frontend, two Go-based microservices (food-catalog-service and order-service), and supporting infrastructure services.

The implementation demonstrates key microservices patterns including service discovery, API gateway routing, containerization, and container orchestration. The application was successfully deployed on a local Kubernetes cluster using Minikube, with all services communicating effectively through Kong's ingress controller.

Key achievements include:

- Successful deployment of a multi-service application using modern cloud-native technologies
- Implementation of service discovery patterns using HashiCorp Consul
- Configuration of Kong API Gateway for intelligent traffic routing
- Containerization of all application components using Docker
- Orchestration using Kubernetes with proper service mesh architecture

---

## **2. Introduction**

### **2.1 Background**

Microservices architecture has become the de facto standard for building scalable, maintainable applications in modern software development. This practical exercise focuses on implementing a complete microservices ecosystem using industry-standard tools and patterns that are widely adopted in production environments.

### **2.2 Project Scope**

The Student Cafe application serves as a practical demonstration of microservices principles, implementing a simple yet comprehensive ordering system where students can browse food items and place orders. The system showcases inter-service communication, service discovery, and API gateway patterns that are fundamental to microservices architecture.

### **2.3 Technology Stack**

- **Frontend:** React.js with TypeScript
- **Backend Services:** Go with Chi router framework
- **Service Discovery:** HashiCorp Consul
- **API Gateway:** Kong
- **Container Runtime:** Docker
- **Orchestration:** Kubernetes (Minikube)
- **Package Management:** Helm

---

## **3. Learning Objectives**

### **3.1 Primary Learning Outcomes**

1. **Microservices Design Patterns:** Understanding and implementing core microservices patterns including service discovery, API gateway, and inter-service communication
2. **Container Orchestration:** Practical experience with Kubernetes deployment, service management, and ingress configuration
3. **Production-Grade Tools:** Hands-on experience with enterprise-level tools used in real-world microservices deployments
4. **DevOps Practices:** Integration of development and deployment workflows using containerization and orchestration

### **3.2 Technical Skills Developed**

- Go programming for microservices development
- React.js frontend development with API integration
- Docker containerization best practices
- Kubernetes resource management and configuration
- Service mesh configuration with Kong and Consul
- Debugging and troubleshooting distributed systems

---

## **4. System Architecture**

### **4.1 High-Level Architecture**

The Student Cafe application follows a distributed microservices architecture with the following key components:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React Frontend │    │   Kong Gateway  │    │ Consul Discovery│
│     (Port 80)   │◄──►│   (Port 80/443) │◄──►│   (Port 8500)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                    ┌───────────┼───────────┐
                    ▼                       ▼
        ┌─────────────────┐        ┌─────────────────┐
        │Food Catalog Svc │        │  Order Service  │
        │   (Port 8080)   │◄──────►│   (Port 8081)   │
        └─────────────────┘        └─────────────────┘
```

### **4.2 Component Descriptions**

#### **4.2.1 Frontend (cafe-ui)**

- **Technology:** React.js with TypeScript
- **Purpose:** User interface for browsing menu items and placing orders
- **Port:** 80 (served via Nginx)
- **Communication:** REST API calls to Kong Gateway

#### **4.2.2 Food Catalog Service**

- **Technology:** Go with Chi router
- **Purpose:** Manages food item inventory and provides menu data
- **Port:** 8080
- **Endpoints:** `/items` (GET), `/health` (GET)

#### **4.2.3 Order Service**

- **Technology:** Go with Chi router
- **Purpose:** Handles order creation and management
- **Port:** 8081
- **Endpoints:** `/orders` (POST), `/health` (GET)

#### **4.2.4 Kong API Gateway**

- **Purpose:** Single entry point for all external traffic with intelligent routing
- **Features:** Path-based routing, load balancing, service discovery integration

#### **4.2.5 Consul Service Discovery**

- **Purpose:** Service registration and discovery for inter-service communication
- **Features:** Health checking, service catalog, distributed configuration

---

## **5. Implementation Process**

### **5.1 Environment Setup**

The implementation began with setting up the local development environment and Kubernetes cluster:

#### **5.1.1 Minikube Cluster Initialization**

```bash
minikube start --cpus 4 --memory 4096
eval $(minikube -p minikube docker-env)
```

![alt text](<screenshots/Start your local Kubernetes cluster.png>)

_Figure 5.1: Minikube cluster initialization showing successful startup and Docker environment configuration_

### **5.2 Microservice Development**

#### **5.2.1 Go Module Initialization**

Before developing the microservices, proper Go module management was established:

![alt text](<screenshots/Before building, initialize Go modules.png>)

_Figure 5.2: Go module initialization for the food catalog service, establishing dependency management_

#### **5.2.2 Order Service Development**

The order service was developed with similar module initialization patterns:

![alt text](<screenshots/Initialize Go modules for order service too.png>)

_Figure 5.3: Order service Go module setup, ensuring consistent dependency management across services_

### **5.3 Project Structure Organization**

The project follows a well-organized structure separating concerns by service:

![alt text](<screenshots/folder structure.png>)

_Figure 5.4: Complete project structure showing separation of microservices, infrastructure configuration, and documentation_

### **5.4 Infrastructure Deployment**

#### **5.4.1 Kubernetes Namespace Creation**

```bash
kubectl create namespace student-cafe
```

![alt text](<screenshots/kubectl create namespace student-cafe.png>)

_Figure 5.5: Kubernetes namespace creation providing isolated environment for the application_

#### **5.4.2 Consul Service Discovery Deployment**

```bash
helm repo add hashicorp https://helm.releases.hashicorp.com
helm install consul hashicorp/consul --set global.name=consul --namespace student-cafe --set server.replicas=1 --set server.bootstrapExpect=1
```

![alt text](<screenshots/Deploy Consul.png>)

_Figure 5.6: Consul deployment using Helm, establishing service discovery infrastructure_

#### **5.4.3 Kong API Gateway Deployment**

```bash
helm repo add kong https://charts.konghq.com
helm repo update
helm install kong kong/kong --namespace student-cafe
```

![alt text](<screenshots/Deploy Kong.png>)

_Figure 5.7: Kong API Gateway deployment, creating the central entry point for all external traffic_

### **5.5 Application Containerization**

#### **5.5.1 Food Catalog Service Containerization**

```bash
docker build -t food-catalog-service:v1 ./food-catalog-service
```

![alt text](<screenshots/docker build -t food-catalog-service:v1 food-catalog-service.png>)

_Figure 5.8: Food catalog service Docker image build process, demonstrating multi-stage build optimization_

#### **5.5.2 Order Service Containerization**

```bash
docker build -t order-service:v1 ./order-service
```

![alt text](<screenshots/docker build -t order-service:v1 order-service.png>)

_Figure 5.9: Order service containerization, following consistent Docker build patterns_

#### **5.5.3 Frontend Containerization**

```bash
docker build -t cafe-ui:v1 ./cafe-ui
```

![alt text](<screenshots/docker build -t cafe-ui:v1 cafe-ui.png>)

_Figure 5.10: React frontend containerization using Nginx for production serving_

### **5.6 Kubernetes Deployment**

#### **5.6.1 Application Services Deployment**

```bash
kubectl apply -f app-deployment.yaml
```

![alt text](<screenshots/kubectl apply -f app-deployment.yaml.png>)

_Figure 5.11: Kubernetes deployment of all application services with proper configuration_

#### **5.6.2 Kong Ingress Configuration**

```bash
kubectl apply -f kong-ingress.yaml
```

![alt text](<screenshots/kubectl apply -f kong-ingress.yaml.png>)

_Figure 5.12: Kong ingress configuration establishing routing rules for path-based traffic distribution_

---

## **6. Technical Analysis**

### **6.1 Service Discovery Implementation**

The implementation uses Consul for service discovery, allowing services to find and communicate with each other dynamically. Each Go service registers itself with Consul on startup, providing:

- **Service Registration:** Automatic registration with health checks
- **Service Discovery:** Dynamic lookup of service endpoints
- **Health Monitoring:** Continuous health checking with automatic deregistration of unhealthy services

### **6.2 API Gateway Pattern**

Kong serves as the API gateway, providing:

- **Unified Entry Point:** Single endpoint for all external traffic
- **Path-based Routing:** Intelligent routing based on URL paths
- **Load Balancing:** Distribution of traffic across service instances
- **Security:** Centralized authentication and authorization point

### **6.3 Container Orchestration**

Kubernetes provides:

- **Service Management:** Automatic service discovery and load balancing
- **Resource Management:** CPU and memory allocation
- **Scaling:** Horizontal and vertical scaling capabilities
- **Health Management:** Automatic restart of failed containers

### **6.4 Inter-Service Communication**

The order service demonstrates inter-service communication by:

- **Service Discovery:** Using Consul to locate the food catalog service
- **HTTP Communication:** RESTful API calls between services
- **Error Handling:** Graceful handling of service unavailability

---

## **7. Challenges and Solutions**

### **7.1 Go Version Compatibility**

**Challenge:** Docker build failures due to Go version mismatch between Dockerfile and go.mod files.

**Solution:** Updated Dockerfiles to use Go 1.23-alpine and synchronized go.mod files to use compatible versions.

### **7.2 Minikube Docker Environment**

**Challenge:** Docker images not found during Kubernetes deployment.

**Solution:** Configured Docker client to use Minikube's Docker daemon using `eval $(minikube -p minikube docker-env)`.

### **7.3 Service Registration Issues**

**Challenge:** Services failing to register with Consul due to hostname resolution issues.

**Solution:** Updated service registration to use Kubernetes service endpoints instead of pod hostnames.

### **7.4 Kong Ingress Configuration**

**Challenge:** Application not accessible through Kong proxy due to ingress misconfiguration.

**Solution:** Properly configured ingress class and path-based routing with correct service references.

---

## **8. Results and Testing**

### **8.1 Application Access**

The deployed application was successfully accessed through Kong's proxy service:

```bash
minikube service -n student-cafe kong-kong-proxy --url
```

![alt text](<screenshots/minikube service -n student-cafe kong-kong-proxy --url.png>)

_Figure 8.1: Kong proxy service URL generation, providing external access to the application_

### **8.2 User Interface Testing**

#### **8.2.1 Initial Application State**

![alt text](<screenshots/student cafe unordered image.png>)

_Figure 8.2: Student Cafe application initial state showing menu items and empty cart_

#### **8.2.2 Order Placement Functionality**

![alt text](<screenshots/student cafe ordered items and placed ordered sucessfilly.png>)

_Figure 8.3: Successful order placement demonstrating end-to-end functionality from frontend through API gateway to backend services_

### **8.3 System Verification**

The testing validated:

1. **Frontend Functionality:** Menu display and cart management
2. **API Gateway Routing:** Proper traffic routing to backend services
3. **Service Discovery:** Successful inter-service communication
4. **Order Processing:** Complete order workflow from cart to confirmation

---

## **9. Critical Evaluation**

### **9.1 Strengths**

1. **Modern Architecture:** Implementation follows current industry best practices for microservices
2. **Scalability:** Architecture supports horizontal scaling of individual services
3. **Maintainability:** Clear separation of concerns enables independent service development
4. **Production Readiness:** Use of enterprise-grade tools (Kong, Consul, Kubernetes)

### **9.2 Areas for Improvement**

1. **Data Persistence:** Current implementation uses in-memory storage
2. **Security:** Authentication and authorization not implemented
3. **Monitoring:** Lacks comprehensive logging and monitoring solutions
4. **Resilience:** Missing resilience patterns (circuit breaker, retry, timeout)

### **9.3 Learning Outcomes Achievement**

The practical successfully achieved all learning objectives:

- ✅ **Microservices Design:** Implemented core patterns and practices
- ✅ **Container Orchestration:** Demonstrated Kubernetes proficiency
- ✅ **Production Tools:** Gained experience with enterprise-level tools
- ✅ **DevOps Integration:** Integrated development and deployment workflows

---

## **10. Conclusion**

This practical exercise provided comprehensive hands-on experience with modern microservices architecture and cloud-native technologies. The successful implementation of the Student Cafe application demonstrates the complexity and benefits of distributed systems architecture.

### **10.1 Key Achievements**

1. **Technical Proficiency:** Developed expertise in Go, React, Docker, and Kubernetes
2. **Architectural Understanding:** Gained deep understanding of microservices patterns
3. **Tool Mastery:** Acquired practical experience with production-grade tools
4. **Problem-Solving Skills:** Successfully resolved various technical challenges

### **10.2 Industry Relevance**

The technologies and patterns implemented in this practical are directly applicable to modern software development environments. The experience gained provides a solid foundation for working with microservices in professional settings.

### **10.3 Future Enhancements**

Future iterations could include:

- Implementation of resilience patterns (Part 2 of the practical)
- Database integration for data persistence
- Monitoring and observability tools
- CI/CD pipeline implementation
- Security enhancements

---

## **11. References**

1. Fowler, M. (2014). _Microservices_. Retrieved from https://martinfowler.com/articles/microservices.html
2. Kong Inc. (2024). _Kong Gateway Documentation_. Retrieved from https://docs.konghq.com/
3. HashiCorp. (2024). _Consul Documentation_. Retrieved from https://www.consul.io/docs
4. Kubernetes Documentation. (2024). Retrieved from https://kubernetes.io/docs/
5. Docker Documentation. (2024). Retrieved from https://docs.docker.com/
6. Go Programming Language. (2024). Retrieved from https://golang.org/doc/
7. React Documentation. (2024). Retrieved from https://reactjs.org/docs/

---

## **12. Appendices**

### **Appendix A: Complete File Structure**

```
student-cafe/
├── food-catalog-service/
│   ├── main.go
│   ├── Dockerfile
│   ├── go.mod
│   └── go.sum
├── order-service/
│   ├── main.go
│   ├── Dockerfile
│   ├── go.mod
│   └── go.sum
├── cafe-ui/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   ├── package.json
│   └── package-lock.json
├── app-deployment.yaml
├── kong-ingress.yaml
├── deploy.sh
├── cleanup.sh
├── instruction.md
├── README.md
└── report.md
```

### **Appendix B: Key Configuration Files**

Detailed configuration files including Kubernetes manifests, Docker configurations, and service implementations are maintained in the project repository for reference and reproducibility.

### **Appendix C: Troubleshooting Guide**

Common issues encountered during development and their solutions are documented for future reference and to assist other developers working with similar technology stacks.

---
**End of Report**

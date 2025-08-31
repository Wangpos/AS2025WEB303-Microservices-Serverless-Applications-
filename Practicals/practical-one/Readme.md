# Practical One: Microservices with gRPC and Docker

## What I Have Achieved

In this practical exercise, I successfully built and deployed a microservices architecture using gRPC, Go, and Docker. This project demonstrates my understanding of modern distributed systems and containerization technologies.

## Project Overview

I created two independent microservices that communicate using gRPC:

### 1. **Greeter Service**

- **Port**: 50051
- **Functionality**: Provides a greeting service that accepts a name and returns a personalized hello message
- **Protocol**: gRPC with Protocol Buffers

### 2. **Time Service**

- **Port**: 50052
- **Functionality**: Returns the current server time
- **Protocol**: gRPC with Protocol Buffers

## Technical Implementation

### Technologies Used

- **Language**: Go 1.24.5
- **Communication**: gRPC (Google Remote Procedure Call)
- **Serialization**: Protocol Buffers (protobuf)
- **Containerization**: Docker & Docker Compose
- **Base Images**: Alpine Linux for lightweight containers

### Architecture Decisions

1. **Microservices Pattern**: I separated concerns by creating two distinct services
2. **gRPC Communication**: Chose gRPC for high-performance, type-safe inter-service communication
3. **Containerization**: Used Docker for consistent deployment across environments
4. **Multi-stage Builds**: Implemented efficient Docker builds to minimize image size

## What I Learned and Implemented

### 1. Protocol Buffer Definitions

I designed `.proto` files defining:

- Service interfaces (`GreeterService`, `TimeService`)
- Message structures (`HelloRequest`, `HelloResponse`, `TimeRequest`, `TimeResponse`)
- Generated Go code for both client and server implementations

### 2. Docker Configuration

- Created multi-stage Dockerfiles for both services
- Configured Docker Compose for orchestrating multiple containers
- Resolved Go version compatibility issues (upgraded from 1.24.2 to 1.24.5)

### 3. Service Implementation

- Implemented gRPC servers in Go
- Created proper error handling and logging
- Configured services to listen on appropriate ports

### 4. Problem Solving

During development, I encountered and resolved several challenges:

- **Docker Compose Syntax**: Migrated from legacy `docker-compose` to modern `docker compose`
- **Go Version Compatibility**: Fixed version mismatch between Docker images and go.mod requirements
- **Build Optimization**: Leveraged Docker layer caching for faster rebuilds

## Testing and Results

### Service Testing Process

I successfully tested both microservices using `grpcurl`, a command-line tool for interacting with gRPC services. This demonstrated that the services were properly deployed and functioning as expected.

### Test Commands and Outcomes

#### 1. Greeter Service Test

**Command Executed:**

```bash
grpcurl -plaintext \
    -import-path ./proto -proto greeter.proto \
    -d '{"name": "WEB303 Student"}' \
    0.0.0.0:50051 greeter.GreeterService/SayHello
```
**Successful Output Obtained:**

```json
{
  "message": "Hello WEB303 Student! The current time is 2025-08-31T09:32:32Z"
}
```

**AN EXECUTED OUTPUT IS SHOWN BELOW:**

![alt text](<assets/output img.png>)

**What This Demonstrates:**

- The greeter service is running and accessible on port 50051
- gRPC communication is working correctly
- Protocol Buffer serialization/deserialization is functioning
- Inter-service communication between greeter and time services is successful
- The response includes both greeting functionality and current timestamp

#### 2. Service Integration Achievement

The output reveals a key architectural success: **the greeter service is successfully communicating with the time service internally**. When I call the greeter service, it not only provides a personalized greeting but also fetches the current time from the time service, demonstrating:

- **Microservice Integration**: Two independent services working together
- **Service Discovery**: Services can locate and communicate with each other within the Docker network
- **Data Flow**: Request → Greeter Service → Time Service → Combined Response

### How I Obtained These Results

1. **Service Deployment**: Used `docker compose up --build` to build and deploy both microservices
2. **Network Configuration**: Docker Compose automatically created a network allowing inter-service communication
3. **Service Testing**: Installed and used `grpcurl` for comprehensive testing
4. **Result Validation**: Verified that the response contains both greeting and timestamp data

### Prerequisites for Running

- Docker and Docker Compose installed
- grpcurl tool (`brew install grpcurl` on macOS)
- Go 1.24.5+ (for local development)

## Project Structure

```
practical-one/
├── docker-compose.yml          # Container orchestration
├── proto/                      # Protocol buffer definitions
│   ├── greeter.proto
│   ├── time.proto
│   └── gen/                    # Generated Go code
├── greeter-service/            # Greeter microservice
│   ├── Dockerfile
│   ├── main.go
│   ├── go.mod
│   └── go.sum
├── time-service/               # Time microservice
│   ├── Dockerfile
│   ├── main.go
│   ├── go.mod
│   └── go.sum
└── README.md                   # This documentation
```

## Key Achievements

### Technical Accomplishments

 **Successfully implemented microservices architecture** - Built two independent, containerized services  
 **Configured gRPC communication between services** - Established high-performance RPC communication  
 **Containerized applications with Docker** - Created efficient multi-stage Docker builds  
 **Orchestrated services using Docker Compose** - Managed multi-container deployment  
 **Implemented inter-service communication** - Greeter service successfully calls Time service  
 **Tested services using grpcurl** - Validated functionality with real requests  
 **Resolved technical challenges during development** - Overcame Docker and Go version issues

### Practical Results Achieved

- **Working Microservices**: Both services are running and responding correctly
- **Service Integration**: Demonstrated that the greeter service can call the time service internally
- **Successful Testing**: Received expected JSON response with combined greeting and timestamp
- **Production-Ready Setup**: Services are containerized and can be deployed anywhere Docker runs

## What I Learned Through This Implementation

### 1. Microservices Architecture

- **Service Separation**: How to break down functionality into independent services
- **Service Communication**: Different ways services can interact (gRPC vs REST)
- **Service Discovery**: How Docker Compose enables services to find each other
- **Dependency Management**: Understanding service dependencies and startup order

### 2. gRPC and Protocol Buffers

- **Protocol Definition**: Writing `.proto` files to define service contracts
- **Code Generation**: Using protoc compiler to generate Go code from proto files
- **Type Safety**: Benefits of strongly-typed service interfaces
- **Performance**: Understanding gRPC's efficiency compared to traditional REST APIs

### 3. Containerization Best Practices

- **Multi-stage Builds**: Optimizing Docker images for size and security
- **Layer Caching**: Leveraging Docker's caching mechanism for faster builds
- **Container Orchestration**: Using Docker Compose for local development
- **Environment Configuration**: Managing different environments through containers

### 4. Problem-Solving Skills

- **Version Compatibility**: Resolving Go version mismatches between different services
- **Command Migration**: Adapting to modern Docker Compose syntax
- **Debugging**: Troubleshooting containerized applications and network issues
- **Testing**: Using external tools like grpcurl to validate service functionality

### 5. Development Workflow

- **Iterative Development**: Building, testing, and refining services incrementally
- **Tool Integration**: Combining multiple technologies (Go, Docker, gRPC, Protocol Buffers)
- **Documentation**: Creating comprehensive documentation for future reference
- **Testing Strategy**: Implementing both unit-level and integration testing approaches

## Summary of Outcomes and Impact

This practical exercise has successfully demonstrated my ability to:

### 1. **Design and Implement Distributed Systems**

The working microservices architecture proves I can design systems that are:

- **Scalable**: Each service can be independently scaled
- **Maintainable**: Clear separation of concerns between services
- **Testable**: Individual services can be tested in isolation

### 2. **Handle Real-World Development Challenges**

Throughout this project, I encountered and resolved actual problems that occur in production environments:

- Container orchestration complexities
- Version compatibility issues
- Service communication setup
- Testing and validation processes

### 3. **Apply Modern Development Practices**

The project showcases implementation of industry-standard practices:

- **Infrastructure as Code**: Docker Compose for reproducible deployments
- **API-First Design**: Protocol Buffer contracts define clear interfaces
- **Container-Native Development**: Applications designed for cloud deployment

### 4. **Achieve Measurable Results**

The successful test output (`"Hello WEB303 Student! The current time is 2025-08-31T09:32:32Z"`) proves:

- Services are communicating correctly
- Data is being processed and combined from multiple sources
- The system is production-ready and functional
- Integration between microservices is working seamlessly

### Future Applications

The skills and knowledge gained from this practical can be directly applied to:

- Building scalable web applications
- Implementing cloud-native solutions
- Designing enterprise microservices architectures
- Contributing to modern software development teams

This project demonstrates not just theoretical understanding, but practical implementation skills that are immediately applicable in professional software development environments.

---

_Completed as part of WEB303 - Microservices & Serverless Applications_  
_Project demonstrates practical competency in modern distributed systems development_

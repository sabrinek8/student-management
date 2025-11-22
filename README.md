# Student Management Application

A comprehensive student management system built with Spring Boot that allows you to manage students, departments, courses, and enrollments. The application provides a RESTful API and is containerized for easy deployment on Kubernetes.

## 📋 Table of Contents

- [Features](#features)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

## ✨ Features

- **Student Management**: Create, read, update, and delete student records
- **Department Management**: Organize students by departments
- **Course Management**: Manage courses with credits and descriptions
- **Enrollment System**: Track student enrollments in courses with grades and status
- **RESTful API**: Well-structured REST endpoints
- **Swagger Documentation**: Interactive API documentation
- **Containerized**: Docker support for easy deployment
- **Kubernetes Ready**: K8s manifests included
- **Infrastructure as Code**: Terraform scripts for AWS EKS deployment

## 🛠 Technology Stack

- **Backend Framework**: Spring Boot 3.5.5
- **Language**: Java 17
- **Database**:
   - H2 (In-memory for development/testing)
   - MySQL (Production ready)
- **ORM**: Spring Data JPA / Hibernate
- **Build Tool**: Maven
- **Documentation**: SpringDoc OpenAPI (Swagger)
- **Testing**: JUnit 5, Mockito
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Infrastructure**: Terraform (AWS EKS)

## 🏗 Architecture

The application follows a layered architecture:

```
Controllers → Services → Repositories → Entities
```

### Core Entities

1. **Student**: Student information including personal details
2. **Department**: Academic departments
3. **Course**: Course information with credits
4. **Enrollment**: Links students to courses with grades and status

### Relationships

- One Department → Many Students
- One Student → Many Enrollments
- One Course → Many Enrollments
- One Enrollment → One Student and One Course

## 📦 Prerequisites

- Java 17 or higher
- Maven 3.9+
- Docker (optional, for containerization)
- Kubernetes cluster (optional, for K8s deployment)
- Terraform (optional, for infrastructure provisioning)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/sabrinek8/student-management.git
cd student-management
```

### 2. Build the Project

```bash
./mvnw clean package
```

Or on Windows:

```bash
mvnw.cmd clean package
```

## ⚙️ Configuration

### Application Properties

The application uses H2 in-memory database by default. Configuration can be found in `src/main/resources/application.properties`:

```properties
# Server Configuration
server.port=8080
server.address=0.0.0.0

# H2 Database
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=

# JPA Configuration
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
```

### Switching to MySQL

Update `application.properties` for production:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/student_db
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
```

## 🏃 Running the Application

### Option 1: Using Maven

```bash
./mvnw spring-boot:run
```

### Option 2: Using JAR

```bash
java -jar target/student-management-0.0.1-SNAPSHOT.jar
```

### Option 3: Using Docker

```bash
# Build the image
docker build -t student-management:latest .

# Run the container
docker run -p 8080:8080 student-management:latest
```

The application will start on `http://localhost:8080`

## 📚 API Documentation

Once the application is running, access the interactive API documentation:

- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **OpenAPI JSON**: http://localhost:8080/v3/api-docs

### Main Endpoints

#### Students
- `GET /students/getAllStudents` - Get all students
- `GET /students/getStudent/{id}` - Get student by ID
- `POST /students/createStudent` - Create new student
- `PUT /students/updateStudent` - Update student
- `DELETE /students/deleteStudent/{id}` - Delete student

#### Departments
- `GET /Depatment/getAllDepartment` - Get all departments
- `GET /Depatment/getDepartment/{id}` - Get department by ID
- `POST /Depatment/createDepartment` - Create new department
- `PUT /Depatment/updateDepartment` - Update department
- `DELETE /Depatment/deleteDepartment/{id}` - Delete department

#### Enrollments
- `GET /Enrollment/getAllEnrollment` - Get all enrollments
- `GET /Enrollment/getEnrollment/{id}` - Get enrollment by ID
- `POST /Enrollment/createEnrollment` - Create new enrollment
- `PUT /Enrollment/updateEnrollment` - Update enrollment
- `DELETE /Enrollment/deleteEnrollment/{id}` - Delete enrollment

## 🧪 Testing

Run unit tests:

```bash
./mvnw test
```

The project includes unit tests for services using JUnit 5 and Mockito.

### Test Coverage

- Department Service Tests
- Mock-based testing
- Repository layer mocking

## 🚢 Deployment

### Kubernetes Deployment

1. **Build and push Docker image**:

```bash
docker build -t sabk8/student-management:latest .
docker push sabk8/student-management:latest
```

2. **Apply Kubernetes manifests**:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

3. **Verify deployment**:

```bash
kubectl get pods
kubectl get services
```

### AWS EKS Deployment with Terraform

1. **Initialize Terraform**:

```bash
cd terraform
terraform init
```

2. **Review the plan**:

```bash
terraform plan
```

3. **Apply infrastructure**:

```bash
terraform apply
```

4. **Configure kubectl**:

```bash
aws eks update-kubeconfig --name mykubernetes --region us-east-1
```

5. **Deploy application**:

```bash
kubectl apply -f ../deployment.yaml
kubectl apply -f ../service.yaml
```

### Infrastructure Details

The Terraform configuration creates:
- VPC with public and private subnets
- Internet Gateway and NAT Gateway
- EKS Cluster (version 1.30)
- Node Group (t3.medium instances)
- Security Groups for cluster and workers
- Load Balancer for external access

## 📁 Project Structure

```
student-management/
├── src/
│   ├── main/
│   │   ├── java/tn/esprit/studentmanagement/
│   │   │   ├── controllers/       # REST controllers
│   │   │   ├── entities/          # JPA entities
│   │   │   ├── repositories/      # Data access layer
│   │   │   ├── services/          # Business logic
│   │   │   └── StudentManagementApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/                      # Unit tests
├── terraform/                     # Infrastructure as Code
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── deployment.yaml                # Kubernetes deployment
├── service.yaml                   # Kubernetes service
├── Dockerfile                     # Docker configuration
├── pom.xml                        # Maven configuration
└── README.md
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is part of an academic exercise at ESPRIT Tunisia.

## 👥 Authors

- **sabrinek8** - [GitHub Profile](https://github.com/sabrinek8)

## 📧 Contact

For questions or support, please open an issue on the GitHub repository.

## 🔍 Troubleshooting

### Common Issues

**Port 8080 already in use**:
```bash
# Change port in application.properties
server.port=8081
```

**Database connection issues**:
- Verify H2 console is accessible at `/h2-console`
- Check database URL and credentials

**Docker build fails**:
```bash
# Ensure mvnw has execute permissions
chmod +x mvnw
```

**Kubernetes pod not starting**:
```bash
# Check pod logs
kubectl logs <pod-name>

# Check pod status
kubectl describe pod <pod-name>
```

## 🔄 Version History

- **v0.0.1-SNAPSHOT** - Initial release
   - Basic CRUD operations
   - Docker and Kubernetes support
   - Terraform infrastructure provisioning

## 🎯 Future Enhancements

- [ ] Add authentication and authorization
- [ ] Implement pagination for list endpoints
- [ ] Add data validation
- [ ] Implement caching
- [ ] Add monitoring and logging
- [ ] Create frontend application
- [ ] Add email notifications
- [ ] Implement file upload for student documents
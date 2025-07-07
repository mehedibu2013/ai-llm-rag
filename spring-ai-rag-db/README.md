# Spring AI RAG with Database Integration

A full-stack application that demonstrates Retrieval-Augmented Generation (RAG) using Spring AI, a React frontend, and a MySQL database. This application allows users to ask natural language questions and get AI-generated SQL queries that retrieve relevant data from your database.

## ✨ Features

### Backend (Spring Boot)
- **RESTful API** for processing natural language questions
- **Ollama Integration** for local LLM inference using Llama3 model
- **Database Operations** with Spring Data JPA and JDBC Template
- **CORS Configuration** for secure frontend-backend communication
- **SQL Query Generation** from natural language using AI
- **HTML Response Formatting** for database results

### Frontend (React)
- **Modern UI** built with Material-UI (MUI)
- **Real-time Interaction** with the AI model
- **Responsive Design** works on all devices
- **Error Handling** with user-friendly messages
- **Loading States** for better UX
- **Debug Mode** for development (shows raw responses)

## 🛠 Prerequisites

Before you begin, ensure you have the following installed:

### Core Requirements
- **Java 17 or higher** - [Download](https://adoptium.net/)
- **Node.js 16+ and npm 9+** - [Download](https://nodejs.org/)
- **MySQL 8.0+** - [Download](https://dev.mysql.com/downloads/mysql/)
- **Maven 3.6.3+** - [Download](https://maven.apache.org/download.cgi)

### AI Requirements
- **Ollama** - [Installation Guide](https://ollama.ai/)
  - After installation, pull the required model:
    ```bash
    ollama pull llama3:latest
    ```

### Development Tools (Recommended)
- **IDE**: IntelliJ IDEA, VS Code, or Eclipse
- **MySQL Workbench** or any MySQL client
- **Postman** or similar API testing tool

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/mehedibu2013/ai-llm-rag.git
cd spring-ai-rag-db
```

### 2. Backend Setup

#### Database Configuration
1. Create a new MySQL database:
   ```sql
   CREATE DATABASE spring_ai;
   ```

2. The database configuration is already set in `spring-ai-rag-db/src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/spring_ai
   spring.datasource.username=root
   spring.datasource.password=mysql
   ```
   
   **Note**: Update the username and password according to your MySQL setup.

3. Create the required tables (the application will auto-create them with sample data):
   ```sql
   -- Students table
   CREATE TABLE Student (
       id BIGINT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255) NOT NULL
   );
   
   -- Courses table
   CREATE TABLE Course (
       id BIGINT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255) NOT NULL
   );
   
   -- Student-Course relationship table
   CREATE TABLE StudentCourse (
       id BIGINT AUTO_INCREMENT PRIMARY KEY,
       student_id BIGINT,
       course_id BIGINT,
       FOREIGN KEY (student_id) REFERENCES Student(id),
       FOREIGN KEY (course_id) REFERENCES Course(id)
   );
   ```

#### Build and Run
1. Build the project:
   ```bash
   cd spring-ai-rag-db
   ./mvnw clean install
   ```

2. Start the backend server:
   ```bash
   ./mvnw spring-boot:run
   ```
   The backend will be available at `http://localhost:8081`

### 3. Frontend Setup

1. Navigate to the frontend directory and install dependencies:
   ```bash
   cd ../frontend
   npm install
   ```

2. Start the development server:
   ```bash
   npm start
   ```
   The frontend will be available at `http://localhost:3000`

### 4. Verify Installation
1. Open your browser and navigate to `http://localhost:3000`
2. You should see the "AI Assistant" interface
3. Try asking a question like "Show me all students" or "List all courses"

## 📁 Project Structure

```
spring-ai-rag-db/
├── frontend/                    # React frontend application
│   ├── public/                 # Static files
│   │   ├── index.html          # Main HTML file
│   │   ├── favicon.ico         # App icon
│   │   ├── manifest.json       # PWA manifest
│   │   └── robots.txt          # SEO robots file
│   ├── src/                    # Source code
│   │   ├── services/           # API service layer
│   │   │   └── api.js          # Axios instance and API calls
│   │   ├── App.js              # Main application component
│   │   ├── App.css             # App-specific styles
│   │   ├── index.js            # Application entry point
│   │   ├── index.css           # Global styles
│   │   └── logo.svg            # App logo
│   ├── package.json            # Frontend dependencies
│   └── README.md               # Frontend documentation
│
└── spring-ai-rag-db/           # Spring Boot backend
    ├── src/
    │   ├── main/java/com/test/rag/db/spring_ai_rag_db/
    │   │   ├── config/         # Configuration classes
    │   │   │   └── WebConfig.java  # CORS configuration
    │   │   ├── SpringAiRagDbApplication.java  # Main application class
    │   │   ├── StudentCourseController.java   # REST controller
    │   │   └── StudentCourseResource.java     # Business logic service
    │   │
    │   └── resources/
    │       ├── application.properties  # Configuration
    │       ├── static/         # Static resources
    │       └── templates/      # Template files
    │
    ├── src/test/java/com/test/rag/db/spring_ai_rag_db/
    │   └── SpringAiRagDbApplicationTests.java  # Test class
    │
    ├── pom.xml                  # Maven dependencies
    ├── mvnw                     # Maven wrapper (Unix)
    └── mvnw.cmd                 # Maven wrapper (Windows)
```

## 🔌 API Endpoints

### AI Endpoints
- `GET /chat/ask?question={your_question}`
  - Process a natural language question and return an AI-generated SQL query result
  - Query Parameters:
    - `question` (required): The natural language question to ask
  - Response: HTML formatted table with query results

### Example Questions
- "Show me all students"
- "List all courses"
- "Which students are enrolled in course 1?"
- "How many students are there?"
- "What courses does student John take?"

## ⚙️ Configuration

### Backend Configuration (`application.properties`)
```properties
# Application name
spring.application.name=spring-ai-rag-db

# Ollama AI Configuration
spring.ai.ollama.chat.options.model=llama3:latest
spring.ai.ollama.embedding.enabled=true
spring.ai.ollama.embedding.options.model=llama3:latest

# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/spring_ai
spring.datasource.username=root
spring.datasource.password=mysql
spring.jpa.hibernate.ddl-auto=update

# Server Configuration
server.port=8081
```

### Frontend Configuration
The frontend is configured to connect to `http://localhost:8081` by default. If you need to change this, update the `API_BASE_URL` in `frontend/src/services/api.js`.

## 🧪 Testing

### Backend Tests
Run the test suite with Maven:
```bash
cd spring-ai-rag-db
./mvnw test
```

### Frontend Tests
Run the frontend test suite:
```bash
cd frontend
npm test
```

## 🐛 Debugging

### Common Issues
1. **Connection Refused**
   - Ensure Ollama is running: `ollama serve`
   - Check if MySQL server is running
   - Verify the ports (8081 for backend, 3000 for frontend) are not in use

2. **Database Connection Issues**
   - Verify database credentials in `application.properties`
   - Check if the database exists and is accessible
   - Ensure the user has proper permissions

3. **AI Model Not Found**
   - Make sure you've pulled the model: `ollama pull llama3:latest`
   - Verify the model name in `application.properties`

4. **CORS Issues**
   - The backend is configured to allow requests from `http://localhost:3000`
   - If using a different port, update `WebConfig.java`

## 🔧 Development

### Backend Development
- Build: `./mvnw clean package`
- Run tests: `./mvnw test`
- Start application: `./mvnw spring-boot:run`
- Debug mode: Add `-Dspring.profiles.active=dev` to enable debug logging

### Frontend Development
- Install dependencies: `npm install`
- Start dev server: `npm start`
- Build for production: `npm run build`
- Run tests: `npm test`

## 🚀 Deployment

### Backend Deployment
1. Build the JAR file:
   ```bash
   ./mvnw clean package
   ```

2. Run the JAR file:
   ```bash
   java -jar target/spring-ai-rag-db-0.0.1-SNAPSHOT.jar
   ```

### Frontend Deployment
1. Build the production version:
   ```bash
   npm run build
   ```

2. Serve the `build` folder using any static file server (nginx, Apache, etc.)

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Spring AI](https://spring.io/projects/spring-ai) for the AI integration
- [Ollama](https://ollama.ai/) for the local LLM capabilities
- [React](https://reactjs.org/) for the frontend framework
- [Material-UI](https://mui.com/) for the UI components
- [MySQL](https://www.mysql.com/) for the database

## 📝 Notes

- The application currently supports only SELECT queries for security reasons
- The AI model generates SQL queries based on the database schema (Student, Course, StudentCourse tables)
- Results are returned as HTML-formatted tables for easy display
- The frontend includes a debug mode that shows raw responses in development 
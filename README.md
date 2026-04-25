<<<<<<< HEAD
# School Attendance Management System

A comprehensive school attendance management system built with Spring Boot (backend) and React (frontend), featuring email and SMS notifications, Docker containerization, and CI/CD pipelines.

## 🚀 Features

### Core Functionality
- **Student Management**: Add, edit, and manage student information
- **Attendance Tracking**: Record daily attendance with multiple status options
- **Teacher Management**: Manage teacher profiles and assignments
- **Class Management**: Organize students into classes and sections

### Notification System
- **Email Notifications**: Automated email alerts for attendance issues
- **SMS Notifications**: SMS alerts via Twilio integration
- **Configurable Alerts**: Customizable notification settings
- **Notification History**: Track all sent notifications

### System Features
- **RESTful API**: Well-documented REST endpoints
- **Security**: Spring Security with basic authentication
- **Database**: MariaDB for production, H2 for development
- **Containerization**: Full Docker support with multi-stage builds
- **CI/CD**: GitHub Actions pipeline with testing and security scans

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React Frontend │    │ Spring Boot API │    │   MariaDB       │
│   (Nginx)        │◄──►│   (Port 8080)   │◄──►│  (Port 3306)    │
│   (Port 80)      │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        │                        │
         └────────────────────────┼────────────────────────┘
                                  │
                    ┌─────────────────┐
                    │  Docker Network │
                    │  school-network │
                    └─────────────────┘
```

## 📋 Prerequisites

- **Docker** (version 20.0+)
- **Docker Compose** (version 2.0+)
- **Git**
- **Node.js 18+** (for local development)
- **Java 17+** (for local development)

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone <repository-url>
cd school-attendance
```

### 2. Configure Environment
```bash
# Copy environment template
cp .env.example .env

# Edit .env with your configuration
nano .env
```

### 3. Deploy the Application

#### Development Environment
```bash
# Start development environment with hot reload and email testing
./scripts/deploy.sh dev
```

**Development Services:**
- Frontend: http://localhost:80
- Backend API: http://localhost:8080
- MailHog (Email testing): http://localhost:8025
- Database: localhost:3306

#### Production Environment
```bash
# Start production environment
./scripts/deploy.sh prod
```

**Production Services:**
- Frontend: http://localhost:80
- Backend API: http://localhost:8080
- Database: localhost:3306

### 4. Verify Deployment
```bash
# Check service health
docker-compose ps

# View logs
docker-compose logs -f
```

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `DB_ROOT_PASSWORD` | MariaDB root password | - | Yes |
| `DB_NAME` | Database name | school_attendance | No |
| `DB_USER` | Database user | attendance_user | No |
| `DB_PASSWORD` | Database password | - | Yes |
| `SMTP_HOST` | SMTP server host | smtp.gmail.com | Yes |
| `SMTP_PORT` | SMTP server port | 587 | No |
| `SMTP_USERNAME` | SMTP username | - | Yes |
| `SMTP_PASSWORD` | SMTP password/app password | - | Yes |
| `TWILIO_ACCOUNT_SID` | Twilio account SID | - | Yes |
| `TWILIO_AUTH_TOKEN` | Twilio auth token | - | Yes |
| `TWILIO_PHONE_NUMBER` | Twilio phone number | - | Yes |
| `ADMIN_PASSWORD` | Admin user password | - | Yes |

### Email Configuration

#### Gmail Setup
1. Enable 2-factor authentication
2. Generate an App Password
3. Use the App Password in `SMTP_PASSWORD`

#### Other SMTP Providers
Update `SMTP_HOST`, `SMTP_PORT`, and credentials accordingly.

### SMS Configuration

1. Sign up at [Twilio Console](https://console.twilio.com/)
2. Get Account SID, Auth Token, and Phone Number
3. Add to environment variables

## 🔧 Development

### Local Development Setup

#### Backend
```bash
cd backend
mvn clean install
mvn spring-boot:run
```

#### Frontend
```bash
cd frontend
npm install
npm start
```

#### Database
```bash
# Start only the database for local development
docker-compose up -d db
```

### Testing

#### Backend Tests
```bash
cd backend
mvn test
```

#### Frontend Tests
```bash
cd frontend
npm test
```

#### Integration Tests
```bash
# Run full test suite
docker-compose -f docker-compose.yml -f docker-compose.dev.yml run --rm backend mvn test
```
## 📦 Docker Commands

### Basic Operations
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f [service_name]

# Rebuild and start
docker-compose up -d --build

# Scale services
docker-compose up -d --scale backend=2
```

### Maintenance
```bash
# Remove all containers and volumes
docker-compose down -v

# Clean up unused images
docker system prune

# View resource usage
docker stats
```

## 💾 Backup and Restore

### Create Backup
```bash
# Create a backup with timestamp
./scripts/backup.sh

# Create a named backup
./scripts/backup.sh my_backup_name
```

### Restore Backup
```bash
# List available backups
./scripts/restore.sh

# Restore a specific backup
./scripts/restore.sh backup_20241201_143000

# Force restore without confirmation
./scripts/restore.sh backup_20241201_143000 --force
```

## 🔒 Security

### Production Security Checklist
- [ ] Change default passwords
- [ ] Use strong database passwords
- [ ] Configure HTTPS (add SSL certificates)
- [ ] Update SMTP credentials
- [ ] Configure firewall rules
- [ ] Regular security updates
- [ ] Monitor logs for suspicious activity

### Security Features
- Non-root container execution
- Security headers in Nginx
- Input validation and sanitization
- Vulnerability scanning in CI/CD
- Database connection security

## 🚀 CI/CD Pipeline

The GitHub Actions pipeline includes:

1. **Testing Phase**
   - Backend unit and integration tests
   - Frontend unit tests
   - Code coverage reporting

2. **Build Phase**
   - Multi-architecture Docker builds
   - Image optimization and caching
   - Container registry publishing

3. **Security Phase**
   - Vulnerability scanning with Trivy
   - SARIF report generation
   - Security advisories

### Pipeline Triggers
- Push to `main` branch
- Push to `develop` branch
- Pull requests to `main`

## 📊 Monitoring

### Health Checks
- Backend: http://localhost:8080/actuator/health
- Frontend: http://localhost:80/health
- Database: Built-in MariaDB health checks

### Logs
```bash
# Application logs
docker-compose logs backend frontend

# Database logs
docker-compose logs db

# Nginx access logs
docker-compose exec frontend tail -f /var/log/nginx/access.log
```

### Metrics
- Spring Boot Actuator endpoints
- Docker container metrics
- Nginx metrics

## 📱 API Documentation

### Authentication
- **Type**: Basic Authentication
- **Username**: admin
- **Password**: Configured via `ADMIN_PASSWORD`

### Core Endpoints

#### Students
- `GET /api/students` - List all students
- `POST /api/students` - Create new student
- `GET /api/students/{id}` - Get student by ID
- `PUT /api/students/{id}` - Update student
- `DELETE /api/students/{id}` - Delete student

#### Attendance
- `GET /api/attendance` - List attendance records
- `POST /api/attendance` - Record attendance
- `GET /api/attendance/student/{studentId}` - Get student attendance
- `GET /api/attendance/date/{date}` - Get attendance by date

#### Notifications
- `GET /api/notifications` - List notification logs
- `POST /api/notifications/email` - Send email notification
- `POST /api/notifications/sms` - Send SMS notification

#### Teachers
- `GET /api/teachers` - List all teachers
- `POST /api/teachers` - Create new teacher
- `PUT /api/teachers/{id}` - Update teacher
- `DELETE /api/teachers/{id}` - Delete teacher

### Response Format
```json
{
  "success": true,
  "data": {},
  "message": "Operation successful",
  "timestamp": "2024-12-01T14:30:00Z"
}
```

## 🐛 Troubleshooting

### Common Issues

#### Backend Won't Start
```bash
# Check database connectivity
docker-compose logs db

# Check backend logs
docker-compose logs backend

# Verify environment variables
docker-compose config
```

#### Frontend Issues
```bash
# Check if backend is accessible
curl http://localhost:8080/actuator/health

# Check frontend logs
docker-compose logs frontend

# Verify Nginx configuration
docker-compose exec frontend nginx -t
```

#### Database Issues
```bash
# Check database status
docker-compose exec db mysqladmin ping

# Access database shell
docker-compose exec db mysql -u attendance_user -p

# Reset database
docker-compose down -v
docker-compose up -d
```

#### Email/SMS Not Working
```bash
# Check backend logs for errors
docker-compose logs backend | grep -i "mail\|sms\|notification"

# Test email configuration
curl -X POST http://localhost:8080/api/notifications/test-email \
  -H "Content-Type: application/json" \
  -d '{"to":"test@example.com","subject":"Test"}'
```

### Performance Optimization

#### Database
```bash
# Analyze slow queries
docker-compose exec db mysql -u root -p -e "SHOW PROCESSLIST;"

# Check database size
docker-compose exec db mysql -u root -p -e "SELECT table_schema, ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) AS 'DB Size in MB' FROM information_schema.tables GROUP BY table_schema;"
```

#### Application
```bash
# Monitor container resources
docker stats

# Check application metrics
curl http://localhost:8080/actuator/metrics
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make changes and test thoroughly
4. Run tests: `npm test` and `mvn test`
5. Commit changes: `git commit -m "Description"`
6. Push to branch: `git push origin feature-name`
7. Create a Pull Request

### Development Guidelines
- Follow existing code style
- Add tests for new features
- Update documentation
- Ensure all CI checks pass

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and questions:
- Create an issue in the repository
- Check the troubleshooting section
- Review the logs for error details

## 🔄 Version History

- **v1.0.0** - Initial release with core functionality
- **v1.1.0** - Added notification system
- **v1.2.0** - Docker containerization and CI/CD
- **v1.3.0** - Enhanced security and monitoring

## 🎯 Roadmap

- [ ] Mobile application (React Native)
- [ ] Advanced reporting and analytics
- [ ] Integration with school management systems
- [ ] Multi-tenant support
- [ ] Advanced notification templates
- [ ] Kubernetes deployment manifests
- [ ] Grafana dashboard integration

## �️ Technology Stack

### Backend
- **Framework**: Spring Boot 3.1.4
- **Database**: MariaDB
- **Build Tool**: Maven
- **Java Version**: 17+
- **Key Dependencies**:
  - Spring Data JPA
  - Spring Web
  - Apache POI (Excel generation)
  - iText (PDF generation)
  - OpenCSV (CSV generation)

### Frontend
- **Framework**: React 18
- **Build Tool**: Vite
- **Styling**: TailwindCSS
- **Routing**: React Router v6
- **State Management**: React Query (TanStack Query)
- **HTTP Client**: Axios
- **Icons**: Lucide React
- **Charts**: Recharts
- **Forms**: React Hook Form

## 📋 Prerequisites

- Java 17 or higher
- Node.js 16 or higher
- MariaDB 10.5 or higher
- Maven 3.6 or higher

## 🔧 Installation & Setup

### Backend Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd school-attendance
   ```

2. **Database Configuration**
   - Create a MariaDB database named `school_attendance`
   - Update database credentials in `src/main/resources/application.properties`
   ```properties
   spring.datasource.url=jdbc:mariadb://localhost:3306/school_attendance
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   ```

3. **Build and Run**
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

4. **Verify Backend**
   - The backend will start on `http://localhost:8080`
   - Check API documentation at `http://localhost:8080/swagger-ui.html` (if Swagger is configured)

### Frontend Setup

1. **Navigate to frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Access Application**
   - The frontend will start on `http://localhost:5173`
   - Default login credentials: admin/admin123

## 📊 Database Schema

### Core Tables
- **schools**: School information and configuration
- **students**: Student records with personal details
- **teachers**: Teacher profiles and contact information
- **subjects**: Subject master data
- **classes**: Class and section information
- **teacher_subjects**: Teacher-subject assignments
- **teacher_classes**: Teacher-class assignments
- **attendance**: Daily attendance records

### Key Relationships
- Students belong to classes and sections
- Teachers can be assigned to multiple subjects and classes
- Attendance records link students with date and status
- All entities are associated with a school

## 🔌 API Endpoints

### Students
- `GET /api/students` - Get all students
- `GET /api/students/{id}` - Get student by ID
- `POST /api/students` - Create new student
- `PUT /api/students/{id}` - Update student
- `DELETE /api/students/{id}` - Delete student

### Teachers
- `GET /api/teachers` - Get all teachers
- `GET /api/teachers/{id}` - Get teacher by ID
- `POST /api/teachers` - Create new teacher
- `PUT /api/teachers/{id}` - Update teacher
- `DELETE /api/teachers/{id}` - Delete teacher

### Attendance
- `GET /api/attendance` - Get attendance records
- `POST /api/attendance` - Mark attendance
- `PUT /api/attendance/{id}` - Update attendance
- `GET /api/attendance/date/{date}` - Get attendance for specific date

### Reports
- `GET /api/reports/csv` - Export CSV report
- `GET /api/reports/excel` - Export Excel report
- `GET /api/reports/pdf` - Export PDF report

## 🎨 Frontend Pages

### Authentication
- **Login Page**: User authentication with form validation

### Dashboard
- **Overview**: Statistics cards with key metrics
- **Charts**: Attendance trends and class-wise data
- **Quick Actions**: Shortcuts to common tasks

### Management Pages
- **Students**: Complete student management with search and filters
- **Teachers**: Teacher management with subject/class assignments
- **Attendance**: Daily attendance marking interface

### Reports
- **Export Options**: CSV, Excel, and PDF export functionality
- **Filters**: Date range, class, section, and report type filters
- **Quick Reports**: Pre-configured report templates

## 📱 Responsive Design

The application is built with a mobile-first approach and supports:
- **Mobile**: 375px and up
- **Tablet**: 768px and up
- **Desktop**: 1024px and up

Key responsive features:
- Collapsible sidebar on mobile
- Responsive tables with horizontal scroll
- Touch-friendly buttons and interactions
- Optimized layouts for all screen sizes

## 🔒 Security Features

- JWT-based authentication
- Protected routes in frontend
- Input validation on both frontend and backend
- SQL injection prevention with parameterized queries
- XSS protection with proper input sanitization

## 🚀 Deployment

### Backend Deployment
1. Build the JAR file: `mvn clean package`
2. Deploy to your server: `java -jar target/school-attendance-0.0.1-SNAPSHOT.jar`

### Frontend Deployment
1. Build for production: `npm run build`
2. Deploy the `dist` folder to your web server

## 🔧 Features

- ✅ Student Management
- ✅ Teacher Management
- ✅ Attendance Tracking
- ✅ Report Generation
- ✅ Role-based Access Control
- ✅ Data Export (PDF, Excel, CSV)

## 📊 Documentation

- [High-Level Design](docs/HLD.md)
- [Low-Level Design](docs/LLD.md)
- [Database Schema](docs/ERD.md)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📄 License

This project is licensed under the MIT License.
=======
# StudentAttendance
An elegant web app to manage student attendance
>>>>>>> 2965d400d65663761efd008241a0bef912cbd5f9

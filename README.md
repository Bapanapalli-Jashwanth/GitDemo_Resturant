# Restaurant Management System

A complete end-to-end Restaurant Management Application with JWT authentication, role-based access control, and comprehensive CRUD operations for restaurants, menus, tables, orders, and staff.

## 🚀 Tech Stack

### Backend
- **Java 17** with **Spring Boot 3.2.3**
- **Spring Data JPA** for database operations
- **Spring Security** with JWT authentication
- **MySQL 8** as database
- **Flyway** for database migrations
- **Lombok** for boilerplate reduction
- **Springdoc OpenAPI** for API documentation
- **SLF4J** for logging

### Frontend
- **React 18** with **TypeScript**
- **Vite** as build tool
- **Tailwind CSS** for styling
- **Axios** for API calls
- **React Router** for navigation
- **React Context API** for state management

### DevOps
- **Docker** & **Docker Compose** for containerization
- **Kubernetes** manifests for orchestration
- **Vercel** for frontend deployment

## 📁 Project Structure

```
restaurant-management/
├── backend/                    # Spring Boot application
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/restaurant/app/
│   │   │   │   ├── controller/      # REST controllers
│   │   │   │   ├── service/         # Business logic
│   │   │   │   ├── repository/      # JPA repositories
│   │   │   │   ├── entity/          # JPA entities
│   │   │   │   ├── dto/             # Data transfer objects
│   │   │   │   ├── security/        # JWT & security config
│   │   │   │   └── exception/       # Exception handlers
│   │   │   └── resources/
│   │   │       ├── application.yml
│   │   │       └── db/migration/    # Flyway migrations
│   │   └── test/                    # Unit tests
│   ├── Dockerfile
│   └── pom.xml
├── frontend/                   # React application
│   ├── src/
│   │   ├── components/         # Reusable components
│   │   ├── pages/              # Page components
│   │   ├── context/            # React Context
│   │   ├── services/           # API services
│   │   └── types/              # TypeScript types
│   ├── Dockerfile
│   ├── nginx.conf
│   └── package.json
├── k8s/                        # Kubernetes manifests
│   ├── config.yaml
│   ├── mysql.yaml
│   └── backend.yaml
├── docker-compose.yml
└── README.md
```

## 🎯 Features

### Authentication & Authorization
- JWT-based authentication
- Role-based access control (ADMIN, MANAGER, WAITER)
- Secure password encoding with BCrypt
- Token-based session management

### Restaurant Management (ADMIN)
- Create, read, update, and delete restaurants
- Paginated restaurant listings
- Multi-restaurant support

### Menu Management (ADMIN)
- Create categories and menu items
- Update item availability and pricing
- Organize items by categories
- CRUD operations for categories and items

### Table Management (ADMIN)
- Create and configure dining tables
- Set table capacity
- Real-time status tracking (FREE, OCCUPIED, BILLING)
- Update table details

### Order Management (WAITER, MANAGER)
- Create orders for tables
- Add multiple items to orders
- Real-time order status updates (PENDING, PREPARING, READY, COMPLETED, CANCELLED)
- Automatic table status management
- Order history per table
- Restaurant-wide order tracking
- Paginated order listings

### Payment Processing
- Multiple payment modes (CASH, CARD, UPI)
- Payment status tracking
- Integration-ready for payment gateways

### Staff Management (ADMIN, MANAGER)
- Create and manage staff accounts
- Assign roles and permissions
- Associate staff with restaurants
- Delete user accounts

### Additional Features
- Comprehensive logging with SLF4J
- Global exception handling
- Input validation
- API documentation with Swagger/OpenAPI
- Responsive UI with modern design
- Real-time status updates
- Pagination support

## 🛠️ Getting Started

### Prerequisites

- **Java 17+** (for backend)
- **Node.js 18+** (for frontend)
- **Docker & Docker Compose** (for containerized setup)
- **MySQL 8** (if running locally without Docker)
- **Maven** (included as Maven Wrapper)

### Option 1: Running with Docker Compose (Recommended)

This is the easiest way to run the entire application with all dependencies.

```bash
# From the root directory
docker-compose up --build
```

**Access Points:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui/index.html

### Option 2: Running Locally (Manual)

#### Database Setup

1. **Install MySQL 8** and create a database:
   ```sql
   CREATE DATABASE restaurant_db;
   ```

2. **Set environment variables** (or update application-dev.yml):
   ```bash
   export DB_HOST=localhost
   export DB_PORT=3306
   export DB_NAME=restaurant_db
   export DB_USER=root
   export DB_PASSWORD=yourpassword
   export JWT_SECRET=your-secret-key-here-min-256-bits
   ```

#### Backend

```bash
cd backend

# For Windows
mvnw.cmd spring-boot:run

# For Linux/Mac
./mvnw spring-boot:run
```

The backend will start on **http://localhost:8080**

#### Frontend

```bash
cd frontend

# Install dependencies
npm install

# Create .env file
echo "VITE_API_BASE_URL=http://localhost:8080/api" > .env

# Start development server
npm run dev
```

The frontend will start on **http://localhost:5173** (Vite default)

## 🐳 Docker Deployment

### Build Images

```bash
# Backend
cd backend
docker build -t restaurant-backend:latest .

# Frontend
cd frontend
docker build -t restaurant-frontend:latest .
```

### Run with Docker Compose

```bash
docker-compose up -d
```

### Stop Services

```bash
docker-compose down
```

## ☸️ Kubernetes Deployment

### Prerequisites
- Kubernetes cluster (minikube, kind, or cloud provider)
- kubectl configured

### Deploy

```bash
# Create namespace
kubectl create namespace restaurant-app

# Apply configurations
kubectl apply -f k8s/config.yaml -n restaurant-app
kubectl apply -f k8s/mysql.yaml -n restaurant-app
kubectl apply -f k8s/backend.yaml -n restaurant-app

# Check status
kubectl get pods -n restaurant-app
kubectl get services -n restaurant-app
```

### Access Services

```bash
# Port forward backend service
kubectl port-forward -n restaurant-app service/restaurant-backend 8080:8080

# Or use LoadBalancer/NodePort based on your setup
```

## 🌐 Vercel Deployment (Frontend)

1. **Push your code to GitHub**

2. **Import project in Vercel:**
   - Go to [Vercel Dashboard](https://vercel.com)
   - Click "New Project"
   - Import your GitHub repository

3. **Configure build settings:**
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`

4. **Set environment variable:**
   - Add `VITE_API_BASE_URL` with your backend URL
   - Example: `https://your-backend-api.com/api`

5. **Deploy!**

## 📚 API Documentation

### Swagger UI
Once the backend is running, access the interactive API documentation at:
```
http://localhost:8080/swagger-ui/index.html
```

### Main Endpoints

#### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login

#### Restaurants (ADMIN)
- `GET /api/restaurants` - List restaurants (paginated)
- `POST /api/restaurants` - Create restaurant
- `PUT /api/restaurants/{id}` - Update restaurant
- `DELETE /api/restaurants/{id}` - Delete restaurant

#### Menu (ADMIN)
- `GET /api/restaurants/{restaurantId}/categories` - Get categories
- `GET /api/categories/{categoryId}/items` - Get items
- `POST /api/menu-categories` - Create category
- `PUT /api/menu-categories/{id}` - Update category
- `DELETE /api/menu-categories/{id}` - Delete category
- `POST /api/menu-items` - Create item
- `PUT /api/menu-items/{id}` - Update item
- `DELETE /api/menu-items/{id}` - Delete item

#### Tables
- `GET /api/tables?restaurantId={id}` - List tables
- `POST /api/tables` - Create table (ADMIN)
- `PUT /api/tables/{id}` - Update table (ADMIN)
- `DELETE /api/tables/{id}` - Delete table (ADMIN)
- `PUT /api/tables/{id}/status` - Update status (MANAGER, WAITER)

#### Orders
- `GET /api/orders?restaurantId={id}&tableId={id}` - List orders (paginated)
- `GET /api/orders/table/{tableId}` - Get table orders
- `POST /api/orders` - Create order (WAITER)
- `PUT /api/orders/{id}/status` - Update status (MANAGER, WAITER)

#### Payments
- `POST /api/payments` - Process payment

#### Staff (ADMIN, MANAGER)
- `GET /api/users` - List all users (ADMIN)
- `GET /api/users/restaurant/{restaurantId}` - List restaurant staff (ADMIN, MANAGER)
- `POST /api/users` - Create user
- `DELETE /api/users/{id}` - Delete user

## 🔐 Default Credentials

The application doesn't come with seeded data. You need to register the first admin user:

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "admin123",
    "role": "ADMIN"
  }'
```

Then login:
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "admin123"
  }'
```

## 🧪 Testing

### Run Backend Tests

```bash
cd backend
./mvnw test
```

### Run Frontend Tests (if configured)

```bash
cd frontend
npm test
```

## 📝 Environment Variables

### Backend

| Variable | Description | Default |
|----------|-------------|---------|
| `DB_HOST` | Database host | localhost |
| `DB_PORT` | Database port | 3306 |
| `DB_NAME` | Database name | restaurant_db |
| `DB_USER` | Database user | root |
| `DB_PASSWORD` | Database password | - |
| `JWT_SECRET` | JWT signing key | (default provided) |

### Frontend

| Variable | Description | Example |
|----------|-------------|---------|
| `VITE_API_BASE_URL` | Backend API URL | http://localhost:8080/api |

## 🏗️ Architecture

### Backend Architecture
- **Controller Layer**: Handles HTTP requests and responses
- **Service Layer**: Contains business logic
- **Repository Layer**: Data access with Spring Data JPA
- **Security Layer**: JWT authentication and role-based authorization
- **Exception Layer**: Global exception handling

### Frontend Architecture
- **Pages**: Full page components
- **Components**: Reusable UI components
- **Services**: API integration layer
- **Context**: Global state management
- **Types**: TypeScript type definitions

## 🔍 Troubleshooting

### Database Connection Issues
```bash
# Check MySQL is running
docker ps | grep mysql

# Check logs
docker-compose logs mysql
```

### Backend Not Starting
```bash
# Check Java version
java -version

# Clean and rebuild
./mvnw clean install
```

### Frontend Issues
```bash
# Clear node modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Check environment variables
cat .env
```

## 📄 License

This project is open source and available for educational purposes.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📧 Support

For issues and questions, please open an issue in the GitHub repository.

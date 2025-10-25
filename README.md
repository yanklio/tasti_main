# Tasti

A modern recipe management application with a full-stack architecture deployed on AWS.

<img width="1629" height="1012" alt="Tasti Application Screenshot" src="https://github.com/user-attachments/assets/be1cd6b0-4fc5-4fa9-be30-9367e11b1ee4" />

<img width="1629" height="1012" alt="Tasti Recipe View" src="https://github.com/user-attachments/assets/47957318-32dd-4bf0-94fc-58e8a6dcd131" />

## Overview

Tasti is a recipe-sharing platform that allows users to create, share, and manage their favorite recipes. The application features:

- 🔐 **User Authentication** - Secure JWT-based authentication with registration and login
- 📝 **Recipe Management** - Full CRUD operations for recipes with image support
- 🖼️ **Image Storage** - S3-based image storage with presigned URLs for secure uploads
- 🎨 **Modern UI** - Angular Material Design with light/dark theme support
- ☁️ **Cloud Infrastructure** - Fully automated AWS deployment using Terraform
- 🐳 **Containerization** - Docker support for local development

## Project Structure

```
tasti_main/
├── tasti-back/          # Django REST API backend
├── tasti-front/         # Angular frontend application
├── tasti_infra/         # Terraform infrastructure code
└── docker-support/      # Docker Compose configuration
```

## Architecture

### Backend ([tasti-back](./tasti-back))

- **Framework**: Django 5.2.5 with Django REST Framework
- **Database**: PostgreSQL 17
- **Authentication**: JWT tokens via django-rest-framework-simplejwt
- **Storage**: AWS S3 with presigned URLs
- **API Documentation**: OpenAPI/Swagger via drf-spectacular

**Key Features:**

- RESTful API with `/api/v1/` endpoints
- Health check endpoint
- Recipe CRUD operations with pagination
- Image upload with S3 integration
- User registration and authentication

📖 [Backend Documentation](./tasti-back/docs/README.md)

### Frontend ([tasti-front](./tasti-front))

- **Framework**: Angular 20.2.2
- **UI Library**: Angular Material Design
- **State Management**: Signals-based reactive state
- **Styling**: Custom CSS with Material theming

**Key Features:**

- Responsive recipe browsing and management
- Image upload with drag-and-drop support
- User authentication with route guards
- Light/dark/system theme support
- Recipe search and filtering

📖 [Frontend Documentation](./tasti-front/README.md)

### Infrastructure ([tasti_infra](./tasti_infra))

- **IaC Tool**: Terraform (AWS Provider ~> 6.16)
- **Cloud Provider**: AWS
- **Architecture**: Serverless with ECS Fargate

**AWS Resources:**

- **Compute**: ECS Fargate for backend containers
- **Storage**: S3 buckets for recipes and frontend assets
- **Database**: RDS PostgreSQL
- **CDN**: CloudFront distribution for frontend
- **Load Balancing**: Application Load Balancer with HTTPS
- **DNS**: Route 53 for domain management
- **Security**: Secrets Manager for credentials, IAM roles

📖 [Infrastructure Documentation](./tasti_infra/README.md)

## Getting Started

### Prerequisites

- **Docker** and **Docker Compose** (for local development)
- **Node.js** 18+ and **npm** (for frontend development)
- **Python** 3.11+ (for backend development)
- **Terraform** 1.2+ (for infrastructure deployment)
- **AWS CLI** configured with appropriate credentials

### Local Development

#### Option 1: Docker Compose (Recommended)

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd tasti_main
   ```

2. **Start all services:**

   ```bash
   cd docker-support
   docker-compose up
   ```

   This starts:

   - Backend API at `http://localhost:8000`
   - Frontend at `http://localhost:4200`
   - PostgreSQL database
   - Localstack for S3 emulation

#### Option 2: Individual Services

**Backend:**

```bash
cd tasti-back
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r src/requirements.txt
cd src
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

**Frontend:**

```bash
cd tasti-front
npm install
ng serve
```

### Environment Configuration

#### Backend (.env)

Create `tasti-back/src/.env`:

```env
DEBUG=True
SECRET_KEY=your-secret-key
DB_NAME=tastidb
DB_USER=postgres
DB_PASSWORD=your-password
DB_HOST=localhost
DB_PORT=5432

# S3 Configuration (use Localstack for development)
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
AWS_STORAGE_BUCKET_NAME=tasti-recipes-local
AWS_S3_ENDPOINT_URL=http://localhost:4566
AWS_S3_VERIFY=False

# CORS
CORS_ALLOWED_ORIGINS=http://localhost:4200
```

#### Frontend (environment.ts)

The development environment is already configured in `tasti-front/src/environments/environment.development.ts`:

```typescript
export const environment = {
  apiUrl: "http://localhost:8000/api/v1/",
};
```

## Deployment

### AWS Deployment with Terraform

1. **Configure AWS credentials:**

   ```bash
   aws configure
   ```

2. **Initialize Terraform:**

   ```bash
   cd tasti_infra
   terraform init
   ```

3. **Review and apply infrastructure:**

   ```bash
   terraform plan
   terraform apply
   ```

4. **Deploy backend Docker image:**

   ```bash
   # Build and push to ECR
   cd ../tasti-back
   aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin <ecr-url>
   docker build -f docker/Dockerfile -t tasti-backend .
   docker tag tasti-backend:latest <ecr-url>:latest
   docker push <ecr-url>:latest
   ```

5. **Deploy frontend to S3:**

   ```bash
   cd ../tasti-front
   ng build --configuration production
   aws s3 sync dist/ s3://tasti-frontend-bucket/
   ```

### Deployment Outputs

After deployment, Terraform provides:

- **Frontend URL**: CloudFront distribution URL (or custom domain)
- **Backend API URL**: ALB DNS name or `api.<domain>`
- **Database Endpoint**: RDS connection string
- **S3 Bucket Names**: For recipes and frontend assets

## API Documentation

The backend API is documented using OpenAPI/Swagger:

- **Development**: `http://localhost:8000/api/schema/swagger-ui/`
- **Production**: `https://api.<your-domain>/api/schema/swagger-ui/`

### Key Endpoints

- **Health Check**: `GET /api/v1/health/`
- **Authentication**: `POST /api/v1/auth/login/`, `POST /api/v1/auth/register/`
- **Recipes**: `GET|POST /api/v1/recipes/`, `GET|PUT|DELETE /api/v1/recipes/{id}/`
- **Image Upload**: Uses presigned S3 URLs for direct upload

📖 [Full API Documentation](./tasti-back/docs/README.md)

## Testing

### Backend Tests

```bash
cd tasti-back/src
python manage.py test
```

### Frontend Tests

```bash
cd tasti-front
npm test
```

## Database Management

### Migrations

```bash
cd tasti-back/src
python manage.py migrate
```

### Seeding Sample Data

```bash
python manage.py seed_db
```

## Security Features

- 🔒 JWT-based authentication with token rotation
- 🛡️ CORS configuration for cross-origin requests
- 🔐 Secrets Manager for sensitive configuration
- 🌐 HTTPS-only in production with SSL certificates
- 🚫 Private S3 buckets with presigned URL access
- 👮 IAM roles with least-privilege access

## Infrastructure Configuration

### Domain Setup

To use a custom domain, configure in `tasti_infra/variables.tf`:

```hcl
variable "domain_name" {
  description = "Domain name for Route 53 and SSL certificate"
  type        = string
  default     = "your-domain.com"
}
```

### Private Access Mode

Enable private access to restrict backend to only CloudFront:

```hcl
variable "enable_private_access" {
  description = "Enable private access mode"
  type        = bool
  default     = true
}
```

## Monitoring and Logs

- **Backend Logs**: CloudWatch Logs for ECS tasks
- **Database Metrics**: RDS CloudWatch metrics
- **CDN Metrics**: CloudFront access logs
- **Application Health**: `/api/v1/health/` endpoint

## License

This project is licensed under the MIT License.

Built with ❤️ using Django, Angular, and AWS

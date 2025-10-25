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

Django REST API backend with PostgreSQL database, JWT authentication, and S3 image storage.

📖 [Backend Documentation](./tasti-back/docs/README.md)

### Frontend ([tasti-front](./tasti-front))

Angular application with Material Design UI and responsive recipe management interface.

📖 [Frontend Documentation](./tasti-front/README.md)

### Infrastructure ([tasti_infra](./tasti_infra))

Terraform-managed AWS infrastructure with ECS Fargate, RDS PostgreSQL, S3 storage, and CloudFront CDN.

📖 [Infrastructure Documentation](./tasti_infra/README.md)

## Getting Started

### Prerequisites

- **Docker** and **Docker Compose** (for local development)
- **Node.js** 18+ and **npm** (for frontend development)
- **Python** 3.11+ (for backend development)
- **Terraform** 1.2+ (for infrastructure deployment)
- **AWS CLI** configured with appropriate credentials

### Quick Start with Docker Compose

```bash
git clone <repository-url>
cd tasti_main/docker-support
docker-compose up
```

Services will be available at:

- Backend API: `http://localhost:8000`
- Frontend: `http://localhost:4200`
- API Documentation: `http://localhost:8000/api/schema/swagger-ui/`

For detailed setup instructions, see the individual project documentation:

- [Backend Setup](./tasti-back/docs/README.md)
- [Frontend Setup](./tasti-front/README.md)
- [Infrastructure Deployment](./tasti_infra/README.md)

## Deployment

See the [Infrastructure Documentation](./tasti_infra/README.md) for complete AWS deployment instructions using Terraform.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Support

For issues and questions:

- � Issues: [GitHub Issues](https://github.com/yanklio/tasti_main/issues)
- 📖 Documentation: See individual project READMEs

---

Built with ❤️ using Django, Angular, and AWS

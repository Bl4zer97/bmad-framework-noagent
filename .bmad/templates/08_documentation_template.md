# Project Documentation

---

## Document Metadata

| Field              | Value                               |
|--------------------|-------------------------------------|
| Project Name       | `{PROJECT_NAME}`                    |
| Version            | `{VERSION}`                         |
| Date               | `{DATE}`                            |
| Author(s)          | `{AUTHOR_NAMES}`                    |
| Tech Lead          | `{TECH_LEAD_NAME}`                  |
| Status             | `{DRAFT / REVIEW / PUBLISHED}`      |

---

## Project Overview

> _A concise description of what this project does, the problem it solves, and who it is for._

**{PROJECT_NAME}** is a {SHORT_DESCRIPTION — e.g., "RESTful API built with ASP.NET Core 8 and Clean Architecture that provides..."}.

| Attribute           | Detail                                              |
|---------------------|-----------------------------------------------------|
| Technology Stack    | .NET 8, ASP.NET Core, EF Core, SQL Server/PostgreSQL|
| Architecture        | Clean Architecture + DDD + CQRS (MediatR)           |
| Cloud Platform      | Microsoft Azure                                     |
| Authentication      | Azure AD / Entra ID (JWT Bearer)                    |
| Repository          | {GITHUB_URL / AZURE_DEVOPS_URL}                     |
| API Documentation   | `{BASE_URL}/swagger` (non-production only)          |
| CI/CD               | {GITHUB_ACTIONS_URL / AZURE_DEVOPS_PIPELINE_URL}    |

---

## Getting Started

### Prerequisites

Ensure the following tools are installed before beginning:

| Tool                                | Version     | Install / Download                                          |
|-------------------------------------|-------------|-------------------------------------------------------------|
| .NET 8 SDK                          | 8.x (LTS)   | https://dotnet.microsoft.com/download/dotnet/8.0            |
| Visual Studio Code                  | Latest      | https://code.visualstudio.com/                              |
| VS Code C# Dev Kit Extension        | Latest      | `ms-dotnettools.csdevkit` (VS Code Marketplace)             |
| VS Code GitHub Copilot Extension    | Latest      | `GitHub.copilot` (VS Code Marketplace)                      |
| Docker Desktop                      | Latest      | https://www.docker.com/products/docker-desktop              |
| Azure CLI                           | Latest      | https://learn.microsoft.com/en-us/cli/azure/install-azure-cli |
| Git                                 | Latest      | https://git-scm.com/                                        |
| SQL Server (local) or Docker        | 2022+       | Docker: `mcr.microsoft.com/mssql/server:2022-latest`        |
| Node.js (for frontend, if applicable)| 20+ LTS    | https://nodejs.org/                                         |

### Clone & Build

```bash
# 1. Clone the repository
git clone {REPO_URL}
cd {REPO_FOLDER}

# 2. Restore NuGet packages
dotnet restore

# 3. Build the solution
dotnet build

# 4. Run all tests
dotnet test

# 5. Run the API locally
cd src/{ProjectName}.Api
dotnet run
```

The API will be available at:
- HTTP:  `http://localhost:{PORT}`
- HTTPS: `https://localhost:{PORT_HTTPS}`
- Swagger UI: `https://localhost:{PORT_HTTPS}/swagger`

### Local Development with Docker Compose

```bash
# Start all infrastructure dependencies (SQL Server, Redis, Azurite)
docker compose -f docker/docker-compose.dev.yml up -d

# Run the API
cd src/{ProjectName}.Api
dotnet run
```

### Database Setup (First Run)

```bash
# Apply EF Core migrations to create/update the local database
cd src/{ProjectName}.Api
dotnet ef database update --project ../Infrastructure/{ProjectName}.Infrastructure.csproj
```

### User Secrets (Local Development)

> Never put real secrets in `appsettings.json`. Use .NET User Secrets for local development.

```bash
# Initialize user secrets for the API project
cd src/{ProjectName}.Api
dotnet user-secrets init

# Set required secrets
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=localhost;Database={ProjectName}_Dev;..."
dotnet user-secrets set "AzureAd:ClientSecret" "{YOUR_DEV_CLIENT_SECRET}"
```

---

## Architecture Overview

> See [Architecture Document](../agents/{agent}/outputs/04_architecture.md) for full diagrams and detail.

This project follows **Clean Architecture** with the following layers:

```
{ProjectName}.sln
├── src/
│   ├── {ProjectName}.Domain/           # Business entities, value objects, domain events
│   ├── {ProjectName}.Application/      # Use cases (CQRS), DTOs, validators, interfaces
│   ├── {ProjectName}.Infrastructure/   # EF Core, repositories, external API clients
│   └── {ProjectName}.Api/              # ASP.NET Core controllers, middleware, startup
└── tests/
    ├── {ProjectName}.Domain.Tests/
    ├── {ProjectName}.Application.Tests/
    ├── {ProjectName}.Infrastructure.Tests/
    └── {ProjectName}.Api.Tests/
```

**Dependency Rule:** Dependencies point inward — `Infrastructure` and `Api` depend on `Application`, which depends on `Domain`. The `Domain` has zero external dependencies.

---

## API Documentation

### Interactive Documentation

When running locally or in non-production environments, Swagger UI is available at:

```
https://localhost:{PORT}/swagger
```

### Authentication

All API endpoints (except `/health`) require a valid JWT Bearer token issued by Azure AD / Entra ID.

```http
Authorization: Bearer {JWT_TOKEN}
```

**Obtaining a token for local development:**

```bash
# Using Azure CLI
az account get-access-token --resource {API_APP_ID_URI} --query accessToken -o tsv

# Using curl with client credentials flow
curl -X POST "https://login.microsoftonline.com/{TENANT_ID}/oauth2/v2.0/token" \
  -d "grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={SECRET}&scope={SCOPE}"
```

### Key Endpoints

| Method | Path                              | Description                          |
|--------|-----------------------------------|--------------------------------------|
| GET    | `/health`                         | Basic health check                   |
| GET    | `/health/ready`                   | Readiness probe (DB + dependencies)  |
| GET    | `/api/v1/{resources}`             | List all {resources} (paged)         |
| POST   | `/api/v1/{resources}`             | Create a new {resource}              |
| GET    | `/api/v1/{resources}/{id}`        | Get {resource} by ID                 |
| PUT    | `/api/v1/{resources}/{id}`        | Update {resource}                    |
| DELETE | `/api/v1/{resources}/{id}`        | Delete {resource}                    |

### Error Responses

All errors follow [RFC 7807 Problem Details](https://www.rfc-editor.org/rfc/rfc7807):

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "Bad Request",
  "status": 400,
  "detail": "One or more validation errors occurred.",
  "errors": {
    "PropertyName": ["Error message"]
  }
}
```

---

## Configuration Guide

### Configuration Hierarchy

Configuration is loaded in this order (later sources override earlier):

1. `appsettings.json` — Base configuration (non-secret, committed to source control)
2. `appsettings.{Environment}.json` — Environment-specific overrides
3. Environment Variables — Set in Azure App Service / Docker
4. Azure Key Vault — Secrets (runtime injection via `IConfiguration`)
5. .NET User Secrets — Local developer overrides (never committed)

### `appsettings.json` Structure

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "Microsoft.EntityFrameworkCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "TenantId": "{TENANT_ID}",
    "ClientId": "{CLIENT_ID}",
    "Audience": "{API_APP_ID_URI}"
  },
  "ConnectionStrings": {
    "DefaultConnection": "{SET_VIA_KEY_VAULT_OR_USER_SECRETS}"
  },
  "KeyVault": {
    "Uri": "https://{VAULT_NAME}.vault.azure.net/"
  },
  "ApplicationInsights": {
    "ConnectionString": "{SET_VIA_KEY_VAULT_OR_ENVIRONMENT}"
  },
  "{ProjectName}": {
    "{SettingGroup}": {
      "{Setting}": "{VALUE}"
    }
  }
}
```

### Azure Key Vault Secrets

| Secret Name                              | Description                              |
|------------------------------------------|------------------------------------------|
| `ConnectionStrings--DefaultConnection`   | Database connection string               |
| `AzureAd--ClientSecret`                  | AAD application client secret            |
| `ApplicationInsights--ConnectionString`  | Application Insights connection string   |
| `{CustomSecret}`                         | {DESCRIPTION}                            |

> Key Vault secret names use `--` as the delimiter, which maps to `:` in `IConfiguration`.

### Azure App Service Configuration

Set the following application settings in the Azure Portal or via Bicep/Terraform:

| Setting Name                         | Source         | Notes                              |
|--------------------------------------|----------------|------------------------------------|
| `ASPNETCORE_ENVIRONMENT`             | App Setting    | `Production` / `Staging`           |
| `KeyVault__Uri`                      | App Setting    | Key Vault URI                      |
| `APPLICATIONINSIGHTS_CONNECTION_STRING` | App Setting | Or via Key Vault reference         |
| `WEBSITE_RUN_FROM_PACKAGE`           | App Setting    | `1` for run-from-package deployment|

---

## Deployment Guide

### Azure App Service Deployment

#### Prerequisites

- Azure subscription with resource group provisioned
- Azure Container Registry (ACR) for Docker images
- Azure App Service plan created
- Azure Key Vault provisioned and secrets populated
- Managed Identity enabled on App Service with Key Vault access

#### Deployment Steps

```bash
# 1. Build and publish the Docker image
docker build -f docker/Dockerfile -t {acr-name}.azurecr.io/{project-name}:{tag} .

# 2. Push to Azure Container Registry
az acr login --name {acr-name}
docker push {acr-name}.azurecr.io/{project-name}:{tag}

# 3. Update App Service to use new image
az webapp config container set \
  --name {app-service-name} \
  --resource-group {rg-name} \
  --docker-custom-image-name {acr-name}.azurecr.io/{project-name}:{tag}

# 4. Apply EF Core migrations (CI/CD pipeline step)
dotnet ef database update \
  --project src/{ProjectName}.Infrastructure \
  --startup-project src/{ProjectName}.Api \
  --connection "{CONNECTION_STRING}"
```

### Dockerfile

```dockerfile
# docker/Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["src/{ProjectName}.Api/{ProjectName}.Api.csproj", "src/{ProjectName}.Api/"]
COPY ["src/{ProjectName}.Application/{ProjectName}.Application.csproj", "src/{ProjectName}.Application/"]
COPY ["src/{ProjectName}.Domain/{ProjectName}.Domain.csproj", "src/{ProjectName}.Domain/"]
COPY ["src/{ProjectName}.Infrastructure/{ProjectName}.Infrastructure.csproj", "src/{ProjectName}.Infrastructure/"]
RUN dotnet restore "src/{ProjectName}.Api/{ProjectName}.Api.csproj"
COPY . .
RUN dotnet build "src/{ProjectName}.Api/{ProjectName}.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "src/{ProjectName}.Api/{ProjectName}.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "{ProjectName}.Api.dll"]
```

### CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.x'
      - run: dotnet restore
      - run: dotnet build --no-restore --configuration Release
      - run: dotnet test --no-build --collect:"XPlat Code Coverage" --results-directory ./coverage
      - uses: codecov/codecov-action@v4
        with:
          directory: ./coverage

  deploy-staging:
    needs: build-and-test
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      # Docker build, push, deploy to staging slot
```

---

## Development Guide

### Adding a New Feature

Follow this pattern to add a new feature in Clean Architecture:

1. **Domain** — Add entity/value object in `{ProjectName}.Domain/Entities/`
2. **Application** — Add command/query + handler in `{ProjectName}.Application/Features/{FeatureName}/`
3. **Infrastructure** — Add EF Core configuration + repository in `{ProjectName}.Infrastructure/`
4. **API** — Add controller endpoint in `{ProjectName}.Api/Controllers/`
5. **Tests** — Add unit tests + integration tests in corresponding test projects
6. **Migration** — Run `dotnet ef migrations add {MigrationName}`

### Useful Commands

```bash
# Run specific test project
dotnet test tests/{ProjectName}.Domain.Tests/

# Run tests with coverage
dotnet test --collect:"XPlat Code Coverage"

# Generate coverage report
reportgenerator -reports:**/coverage.cobertura.xml -targetdir:coverage-report -reporttypes:Html

# Add EF Core migration
dotnet ef migrations add {MigrationName} \
  --project src/{ProjectName}.Infrastructure \
  --startup-project src/{ProjectName}.Api

# Remove last migration
dotnet ef migrations remove \
  --project src/{ProjectName}.Infrastructure \
  --startup-project src/{ProjectName}.Api

# List outdated packages
dotnet list package --outdated

# Check for vulnerable packages
dotnet list package --vulnerable

# Format code
dotnet format
```

### Code Conventions

- Use `record` types for Commands, Queries, and DTOs
- Use primary constructors (C# 12) where appropriate
- All public types and members must have XML documentation comments
- Use `CancellationToken` in all async method signatures
- Prefer `IReadOnlyList<T>` over `List<T>` in return types
- Use `nameof()` instead of string literals for property names
- Use `ArgumentNullException.ThrowIfNull()` for guard clauses

---

## Troubleshooting

### Common Issues

#### `dotnet run` fails with certificate error

```bash
# Trust the ASP.NET Core development certificate
dotnet dev-certs https --trust
```

#### EF Core migration fails

```bash
# Verify connection string
dotnet user-secrets list

# Re-run migration with verbose output
dotnet ef database update --verbose

# Check if migration is already applied
dotnet ef migrations list
```

#### Azure AD authentication failure (401)

- Verify `TenantId`, `ClientId`, and `Audience` in `appsettings.json` / user secrets
- Ensure the JWT token audience matches the API's `ClientId` or `App ID URI`
- Check token expiry: Azure AD tokens expire after 1 hour by default

#### Application Insights not receiving telemetry

- Check `APPLICATIONINSIGHTS_CONNECTION_STRING` is set correctly
- Ensure `AddApplicationInsightsTelemetry()` is called in `Program.cs`
- Verify firewall/network rules allow outbound HTTPS to `dc.applicationinsights.azure.com`

#### Docker build fails

```bash
# Clean Docker build cache
docker build --no-cache -f docker/Dockerfile .

# Check .dockerignore is not excluding required files
cat .dockerignore
```

---

## Changelog

All notable changes to this project are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

### [{VERSION}] — {DATE}

#### Added
- {NEW_FEATURE_OR_CAPABILITY}
- {NEW_ENDPOINT}

#### Changed
- {BREAKING_OR_NON_BREAKING_CHANGE}

#### Fixed
- {BUG_FIX}

#### Security
- {SECURITY_FIX}

---

### [1.0.0] — {DATE}

#### Added
- Initial release
- {FEATURE_1}
- {FEATURE_2}
- Clean Architecture solution structure
- EF Core code-first migrations
- Azure AD authentication
- Health check endpoints
- Swagger / OpenAPI documentation
- CI/CD pipeline (GitHub Actions / Azure DevOps)

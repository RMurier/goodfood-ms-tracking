# goodfood-ms-tracking

Real-time delivery tracking microservice for the [GoodFood](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF) platform.

**Status:** 🚧 Scaffold — this is currently an unmodified `dotnet new webapi` template (only the sample `/weatherforecast` endpoint). No tracking logic has been written yet. See [`goodfood-ms-auth`](https://github.com/RMurier/goodfood-ms-auth#readme) for what a fleshed-out service in this platform looks like.

## Table of Contents

- [Intended Purpose](#intended-purpose)
- [Tech Stack](#tech-stack)
- [Environment Variables](#environment-variables)
- [Running Locally](#running-locally)
- [Tests](#tests)
- [CI/CD](#cicd)

## Intended Purpose

Track delivery status/position in near real time between `ms-commandes` marking an order ready and it reaching the customer. The only service on MongoDB rather than SQL Server — a natural fit for high-write, loosely-structured location/status events.

## Tech Stack

- .NET 9 / ASP.NET Core Web API
- MongoDB (driver not yet added — connection string is wired up, no data access code exists yet)

## Environment Variables

| Variable | Description |
|----------|--------------|
| `ASPNETCORE_ENVIRONMENT` | `Development` or `Production` |
| `ConnectionStrings__MongoDB` | MongoDB connection string (database `GoodFood_Tracking_Dev` in dev) |

## Running Locally

### Via the platform's docker-compose

From the [parent repo](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF):

```bash
docker compose -f docker-compose.dev.yml up -d db-mongo-dev ms-tracking-dev
```

Runs on `http://localhost:3005`, Swagger at `http://localhost:3005/swagger`.

### Standalone

```bash
cd GoodFood.Tracking.Api
dotnet restore
dotnet run
```

## Tests

```bash
dotnet test GoodFood.Tracking.Tests/GoodFood.Tracking.Tests.csproj --verbosity normal
```

Currently a single sanity check ([`SanityTests.cs`](GoodFood.Tracking.Tests/SanityTests.cs)), there to keep the CI test job green while the service is empty.

## CI/CD

Built, scanned (SonarQube, Trivy, OWASP Dependency-Check, GitGuardian) and published on every push, gated on all of them passing — see the [parent repo's CI/CD Pipeline section](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF#cicd-pipeline) for how the pipeline is wired across repos.

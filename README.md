# TaskManagementPortal

A Clean Architecture .NET 10 solution for task management with Categories, Statuses, and Tasks.

## 🏗 Architecture

```
TaskManagementPortal.sln
├── src/
│   ├── TaskManagementPortal.Api/           # ASP.NET Core Web API (Presentation)
│   ├── TaskManagementPortal.Application/   # Use cases, Services, Validators, DTOs
│   ├── TaskManagementPortal.Core/          # Domain Entities, Interfaces, Enums
│   └── TaskManagementPortal.Infrastructure/# EF Core, Repositories, DB Context
├── tests/
│   ├── TaskManagementPortal.Api.Tests/
│   ├── TaskManagementPortal.Application.Tests/
│   └── TaskManagementPortal.Infrastructure.Tests/
├── database/
│   ├── migrations/                         # EF Core migrations
│   └── scripts/
│       ├── main/                           # Production-approved (immutable)
│       └── develop/                        # Work-in-progress
└── frontend/                               # Vanilla JS (planned)
```

## 🔄 Dependency Flow

```
Api → Application → Core ← Infrastructure
```

## 📋 Database Schema

| Table | Description |
|-------|-------------|
| **Categories** | Task categories (Id, Name, Description, Color, IsActive) |
| **Statuses** | Task statuses (Id, Name, DisplayName, Color, SortOrder, IsActive, IsDefault, IsCompleted) |
| **Tasks** | Tasks (Id, Title, Description, CategoryId, StatusId, Priority, DueDate, timestamps) |
| **vw_TaskDetails** | View joining Tasks, Categories, Statuses |
| **sp_GetTasksPaged** | Stored procedure for filtered/sorted/paged tasks |
| **fn_GetTaskStatsByCategory** | Function for category statistics |

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| **Framework** | .NET 10 (Preview) |
| **API** | ASP.NET Core Web API |
| **ORM** | Entity Framework Core 9.0 |
| **Database** | SQL Server |
| **Validation** | FluentValidation 12 |
| **Mapping** | Mapster 10 |
| **Logging** | Serilog |
| **API Docs** | Swashbuckle (Swagger/OpenAPI) |
| **API Versioning** | Microsoft.AspNetCore.Mvc.Versioning |
| **Testing** | xUnit, FluentAssertions, Moq |
| **Frontend** | Vanilla ES Modules (no build) |

## 🚀 Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0) (Preview)
- [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (LocalDB, Express, or full)
- [Visual Studio 2022 17.10+](https://visualstudio.microsoft.com/) or [VS Code](https://code.visualstudio.com/)

### Setup

```bash
# Clone repository
git clone https://github.com/your-org/TaskManagementPortal.git
cd TaskManagementPortal

# Restore packages
dotnet restore

# Build solution
dotnet build

# Run API (from src/TaskManagementPortal.Api)
dotnet run --project src/TaskManagementPortal.Api
```

### Database Setup

1. Update connection string in `src/TaskManagementPortal.Api/appsettings.json`
2. Run migrations:
   ```bash
   dotnet ef database update --project src/TaskManagementPortal.Infrastructure --startup-project src/TaskManagementPortal.Api
   ```

## 🌿 Git Branch Strategy

| Branch | Purpose | Protection | SQL Scripts |
|--------|---------|------------|-------------|
| `main` | Production-ready | PR required, 1+ approval, status checks | `database/scripts/main/` |
| `develop` | Integration | PR required, build check | `database/scripts/develop/` |
| `feature/*` | Feature work | Short-lived, from `develop` | Local → `develop` on PR |
| `hotfix/*` | Emergency fixes | From `main`, merge to both | Update `main` scripts |

### Workflow

```bash
# 1. Create feature branch from develop
git checkout develop
git pull
git checkout -b feature/my-feature

# 2. Work, commit, push
git add .
git commit -m "feat: add my feature"
git push origin feature/my-feature

# 3. PR: feature/* → develop (squash merge)

# 4. PR: develop → main (copy SQL scripts develop/ → main/)

# 5. Tag release on main
git tag v1.0.0
git push origin v1.0.0
```

## 📁 SQL Script Organization

```
database/scripts/
├── main/                          # Immutable, production-approved
│   ├── 0001_initial_schema.sql
│   ├── 0002_seed_data.sql
│   └── ...
└── develop/                       # Work-in-progress
    ├── 0003_add_new_table.sql
    └── ...
```

**Rule**: Never edit files in `main/` directly. New migrations go to `develop/`. On `develop → main` PR, scripts are copied.

## 🧪 Testing

```bash
# Run all tests
dotnet test

# Run with coverage
dotnet test --collect:"XPlat Code Coverage"

# Run specific project tests
dotnet test tests/TaskManagementPortal.Api.Tests
dotnet test tests/TaskManagementPortal.Application.Tests
dotnet test tests/TaskManagementPortal.Infrastructure.Tests
```

## 📦 NuGet Packages (Managed via Directory.Build.props)

| Package | Version | Projects |
|---------|---------|----------|
| Microsoft.EntityFrameworkCore | 9.0.3 | Infrastructure, Application |
| Microsoft.EntityFrameworkCore.SqlServer | 9.0.3 | Infrastructure |
| Microsoft.EntityFrameworkCore.Tools | 9.0.3 | Infrastructure |
| FluentValidation | 12.2.0 | Application |
| Mapster | 10.0.14 | Core, Infrastructure, Application |
| Mapster.DependencyInjection | 1.0.1 | Api |
| Serilog.AspNetCore | 9.0.0 | Api |
| Swashbuckle.AspNetCore | 8.1.1 | Api |
| Microsoft.AspNetCore.Mvc.Versioning | 5.1.0 | Api |
| xUnit | 2.9.3 | All Tests |
| FluentAssertions | 8.1.1 | All Tests |
| Moq | 4.20.72 | All Tests |

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'feat: add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

---

**Built with .NET 10 and Clean Architecture principles**
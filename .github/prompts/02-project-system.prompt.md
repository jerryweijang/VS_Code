# AI RAG System Development Plan (Blazor Server Architecture)

This document outlines the comprehensive development plan for an AI Retrieval Augmented Generation (RAG) System using Blazor Server architecture with Oracle Database integration.

## Project Overview

The AI RAG System will be built on the following key technologies and principles:

1. **Blazor Server Architecture** - For a responsive, server-side rendered web application
2. **Oracle Database** - Using Oracle.ManagedDataAccess.Client for data persistence
3. **Windows Authentication** - For secure user authentication
4. **Dependency Injection** - Following SOLID principles for maintainable code
5. **Transaction Management** - Using Oracle Stored Procedures

## Development Steps

### Phase 1: Environment Setup and Project Creation

1. Verify the development environment prerequisites:
   ```ps1
   git --version
   dotnet --version  # Ensure .NET SDK version > 9.0.100
   ```

2. Create a new Blazor Server project:
   ```ps1
   dotnet new blazorserver -n AIRagSystem
   cd AIRagSystem
   ```

3. Setup initial source control:
   ```ps1
   dotnet new gitignore
   dotnet new editorconfig
   ```

4. Add a `.gitattributes` file:
   ```ps1
   # Create .gitattributes file with appropriate settings
   # (Content as specified in standard template)
   ```

5. Create a `GlobalUsings.cs` file for common namespaces:
   ```ps1
   # Add global using statements for commonly used namespaces
   ```

### Phase 2: Database Integration Setup

1. Add required NuGet packages:
   ```ps1
   dotnet add package Oracle.ManagedDataAccess.Core
   dotnet add package Microsoft.Extensions.Configuration
   dotnet add package Microsoft.Extensions.DependencyInjection
   ```

2. Configure `appsettings.json` with connection strings:
   ```json
   {
     "ConnectionStrings": {
       "OracleConnection": "User Id=username;Password=password;Data Source=//hostname:port/service_name;"
     }
   }
   ```

3. Create Database Service layer:
   - Implement interface-based design for Oracle DB access
   - Create models for database entities
   - Implement repository pattern for data access
   - Design stored procedure calls with appropriate transaction management

### Phase 3: Authentication and Security

1. Configure Windows Authentication:
   - Update `Program.cs` to enable Windows Authentication
   - Configure appropriate authorization policies
   - Implement user identity management

2. Implement security best practices:
   - Secure connection strings
   - Implement proper error handling and logging
   - Setup security headers and HTTPS

### Phase 4: Dependency Injection and Service Configuration

1. Configure DI in `Program.cs`:
   ```csharp
   // Register database services
   builder.Services.AddScoped<IDatabaseService, OracleDatabaseService>();
   
   // Register email service
   builder.Services.AddScoped<IEmailService, EmailService>();
   
   // Register logging
   builder.Services.AddLogging(builder => 
   {
       builder.AddConsole();
       builder.AddDebug();
       // Add any other logging providers as needed
   });
   ```

2. Implement service interfaces and implementations:
   - Database service (IDatabaseService, OracleDatabaseService)
   - Email service (IEmailService, EmailService)
   - Utilize ILogger<T> in all services

### Phase 5: RAG Implementation

1. Design and implement RAG components:
   - Document ingestion and processing pipeline
   - Vector database integration (if applicable)
   - Embedding generation for text chunks
   - Retrieval mechanisms
   - Integration with chosen AI model

2. Create service layer for RAG operations:
   - Document processing service
   - Query service
   - Response generation service

### Phase 6: User Interface Development

1. Design and implement Blazor components:
   - Layout and navigation structure
   - Search interface
   - Results display
   - Document management interface

2. Implement responsive design and accessibility features

### Phase 7: Testing

1. Unit testing:
   - Database service tests
   - RAG component tests
   - Authentication tests

2. Integration testing:
   - End-to-end workflow tests
   - Performance testing

### Phase 8: Deployment Planning

1. Define deployment strategy:
   - Server requirements
   - Database migration plan
   - Configuration management
   - Monitoring setup

## Code Structure

```
AIRagSystem/
├── Data/
│   ├── Entities/           # Database entity models
│   ├── Repositories/       # Repository implementations
│   └── StoredProcedures/   # Stored procedure definitions
├── Services/
│   ├── Database/           # Database service implementation
│   ├── Email/              # Email service implementation
│   └── RAG/                # RAG service implementations
├── Pages/                  # Blazor pages
├── Shared/                 # Shared Blazor components
├── wwwroot/                # Static assets
├── Program.cs              # Application entry point and DI configuration
├── GlobalUsings.cs         # Global using statements
└── appsettings.json        # Application configuration
```

## Implementation Details

### Program.cs Configuration

```csharp
using Microsoft.AspNetCore.Authentication.Negotiate;
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using AIRagSystem.Services.Database;
using AIRagSystem.Services.Email;
using AIRagSystem.Services.RAG;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Configure Windows Authentication
builder.Services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
builder.Services.AddAuthorization(options =>
{
    options.FallbackPolicy = options.DefaultPolicy;
});

// Configure Database Services
builder.Services.AddScoped<IDatabaseService, OracleDatabaseService>();

// Configure Email Service
builder.Services.AddScoped<IEmailService, EmailService>();

// Configure RAG Services
builder.Services.AddScoped<IDocumentProcessingService, DocumentProcessingService>();
builder.Services.AddScoped<IQueryService, QueryService>();
builder.Services.AddScoped<IResponseGenerationService, ResponseGenerationService>();

// Configure Logging
builder.Services.AddLogging(loggingBuilder =>
{
    loggingBuilder.AddConsole();
    loggingBuilder.AddDebug();
    // Add any other logging providers as needed
});

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthentication();
app.UseAuthorization();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host");

app.Run();
```

### Database Service Implementation

```csharp
public interface IDatabaseService
{
    Task<T> ExecuteStoredProcedureAsync<T>(string procedureName, Dictionary<string, object> parameters);
    Task<int> ExecuteNonQueryAsync(string procedureName, Dictionary<string, object> parameters);
    Task<bool> BeginTransactionAsync();
    Task<bool> CommitTransactionAsync();
    Task<bool> RollbackTransactionAsync();
}

public class OracleDatabaseService : IDatabaseService
{
    private readonly string _connectionString;
    private readonly ILogger<OracleDatabaseService> _logger;
    private OracleConnection _connection;
    private OracleTransaction _transaction;

    public OracleDatabaseService(IConfiguration configuration, ILogger<OracleDatabaseService> logger)
    {
        _connectionString = configuration.GetConnectionString("OracleConnection");
        _logger = logger;
    }

    // Implementation of interface methods
    // ...
}
```

## SOLID Principles Implementation

1. **Single Responsibility Principle**
   - Each service has a single purpose (database access, email, RAG operations)
   - Classes are focused on specific tasks

2. **Open/Closed Principle**
   - Use of interfaces allows extension without modification
   - Plugin architecture for components like logging providers

3. **Liskov Substitution Principle**
   - Service implementations can be substituted without affecting functionality
   - Mock implementations can be used for testing

4. **Interface Segregation Principle**
   - Focused interfaces prevent clients from depending on methods they don't use
   - Specialized interfaces for specific functionality

5. **Dependency Inversion Principle**
   - High-level modules depend on abstractions
   - Dependency injection throughout the application

## Next Steps

1. Create detailed technical specifications for each component
2. Establish development timeline and milestones
3. Define testing strategy and acceptance criteria
4. Document API endpoints and service interfaces
5. Develop monitoring and maintenance plan

This development plan provides a structured approach to building the AI RAG System using Blazor Server with Oracle Database integration, following SOLID principles and implementing the requested technical requirements.

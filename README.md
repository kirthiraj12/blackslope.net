# BlackSlope.NET

## What is it?

You can read more in the following blog posts:

* [Introducing BlackSlope: A DotNet Core Reference Architecture from Slalom Build](https://medium.com/slalom-build/introducing-black-slope-a-dotnet-core-reference-architecture-from-slalom-build-3f1452eb62ef)
* [BlackSlope: A Deeper Look at the Components of our DotNet Reference Architecture](https://medium.com/slalom-build/blackslope-a-deeper-look-at-the-components-of-our-dotnet-reference-architecture-b7b3a9d6e43b)
* [BlackSlope in Action: A Guide to Using our DotNet Reference Architecture](https://medium.com/slalom-build/blackslope-in-action-a-guide-to-using-our-dotnet-reference-architecture-d1e41eea8024)

## Installation Instructions

### Install .NET Core
Install the latest verison of .NET Core for Windows/Linux or Mac.
* https://dotnet.microsoft.com/download

### Build (Application)

	dotnet build src/BlackSlope.NET.sln

### Build (Database)

1. Install SQL Server Developer 2019
    > https://www.microsoft.com/en-us/sql-server/sql-server-downloads
2. Update connection string server name and credentials in [appsettings.json](./src/BlackSlope.Api/appsettings.json)
    ```
    MoviesConnectionString
    ```
3. Open PowerShell to your repository root directory and run the following command:
    ```
    dotnet ef database update --project src/BlackSlope.Hosts.Api/BlackSlope.Hosts.Api.csproj
    ```
4. If successful, the result of the above command will be similar to the following example:
    ```
    Build started...
    Build succeeded.
    Applying migration '20190814225754_initialized'.
    Applying migration '20190814225910_seeded'.
    Done.
    ```

### Run

	dotnet run --project src/BlackSlope.Api/BlackSlope.Api.csproj

### Test

    dotnet test ./src/

### Swagger
Open your browser and navigate to ```http://localhost:51385/swagger``` to view the API documentation

### StyleCop and NetAnalyzers
Blackslope makes use of two different analyzers to keep the codebase clean and formatted.
1. StyleCop - for style formatting and code cleanliness
   - Consumes `stylecop.json` files at the project level
   - May be set as part of `.editorconfig`, but documentation is sparse and not recommended at this time
2. Microsoft.CodeAnalysis.NetAnalyzers - Nuget package for the IDE level; covers style formatting and code analysis issues.
   - Consumes `.editorconfig` files set at the solution or project level

**NOTE:** SA and CA rules are globally suppressed at `BlackSlope.Api.Common.GlobalSuppressions`

* [CodeAnalysis FAQ](https://github.com/MicrosoftDocs/visualstudio-docs/issues/2382)
* [When to Use NetAnalyzers?](https://github.com/MicrosoftDocs/visualstudio-docs/issues/2382)
* [.editorconfig Configuration](https://github.com/dotnet/roslyn-analyzers/blob/main/docs/Analyzer%20Configuration.md)
* [stylecop.json Configuration](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/Configuration.md)

**StyleCop**  
The following rules are currently ignored:

| Rule Id | Rule Title |
| --- | --- |
| SA1101 | Prefix local calls with this |
| SA1309 | Field names should not begin with an underscore |
| SA1600 | Elements should be documented |
| SA1614 | Element parameter documentation must have text |
| SA1616 | Element return value documentation must have text |
| SA1629 | Documentation text should end with a period |
| SA1633 | File should have header |
  
**CodeAnalysis**  
The following rules are currently ignored:

| Rule Id | Rule Title | Scope |
| --- | --- | --- |
| CA1031 | Do not catch general exception types | `~M:BlackSlope.Api.Common.Middleware.ExceptionHandling.ExceptionHandlingMiddleware.Invoke(Microsoft.AspNetCore.Http.HttpContext)~System.Threading.Tasks.Task")` |
| CA1710 | Identifiers should have correct suffix | ```~T:BlackSlope.Api.Common.Validators.CompositeValidator\`1``` |
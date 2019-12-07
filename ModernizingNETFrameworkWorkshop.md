# Migrating to .NET Core

### Small Plug 

.NET Foundation is the community that drives the development of .NET. .NET Foundation offers a membership which gives you the opportunity to have a voice in the direction, among other benefits. The only requirement for joining is that you make some contribution to .NET (either through code, docs, or by teaching classes or demos about .NET). There is an optional, annual membership fee.

[.NET Foundation Membership](https://members.dotnetfoundation.org/
)

## Agenda

1. Presentation (W)
1. Demo Application
1. Modernizing
1. Explore .NET Core topics as groups

## Why update to .NET Core?

* Performance
   * [TechEmpower](https://www.techempower.com/benchmarks/) benchmarks - same tests run against many web hosting frameworks
      * .NET Framework is around 100ish down in req/sec
      * .NET Core is number 10 in req/sec (a lot of theoretical, as opposed to practical, frameworks above 10th position)
      * .NET Core 6.97M req/sec vs Node 0.83M vs Java Servlet 2.55M (plain text requests and responses)
   * .NET Core 2.1 increased Bing performance by 34%
   * Do a lot more with a lot less [hardware]
* Platforms
   * Run and develop .NET Core in a lot more places
   * .NET Core 3 supports IoT, *Windows Desktop*, and AI
* Community
   * 87% of contributers are outside of Microsoft
   * 61000+ accepted PRs from community (dated)
   * Contributing is encouraged, lots of guidance along the way
* Other 'Goodies'
   * Kestrel
      * Cross platform web server that can host .NET applications
      * Enables self-hosting options
   * HTTP/2
      * Reduce latency by enabling multiplexing of requests/responses
      * Better error handling
      * More efficient compression
      * *Cannot utilize HTTP/2 with .NET Framework*, can with .NET Core
   * Single .EXEs
      * Entire application can be published into a single EXE rather than a collection of DLLs (including the .NET Core runtime)
   * Built-in DI
      * Pretty robust, will handle the majority of use cases
   * Health Checks
      * Built in as middleware
   * Razor Pages
      * Stand alone pages, not tied to MVC architectures (no controllers needed)
   * Razor Components
      * Allows you to create componentized, stand alone pieces that can be put together to create pages
   * BlAzOr
      * Compile code to WebAssembly
      * Combines razor pages/components and allows them to be used client side
      * Web app now runs in browser
   * Websockets and SignalR
      * Push to browser
      * .NET Core has support for websockets (preferred mode of communication)
      * .NET Framework stuck with long-polling client side
   * Self-Hosted Apps
   * Containerization
      * Smaller, more efficient containers (no Windows needed)
      * Easier hosting
      * Less hardware
      * More consistent builds and deployments

## .NET 5

Release date of ~November 2020, it also marks the end of .NET Framework. .NET 5 is .NET Core. .NET Framework will be supported, but no newer versions. Any apps that are expected to be improved on, not just maintained until EoL, are encouraged to be upgraded to .NET 5. 

### What's not in .NET 5?

* WebForms
* WCF (.NET Core has a client for interacting with providers, but not a way to host a WCF service)
   * Alternatives exist ([see GRPC](https://grpc.io/))

## Why would you not upgrade?

* If you need support for WebForms or WCF, you will need to stick with Framework until those technologies are retired
* Not a 1:1 upgrade
   * There are different APIs which requires some planning and analysis and takes time.
   * Cost needs to be evaluated
* Monoliths
   * Increases cost and need for planning/analysis
   * Look for opportunities to upgrade rather than challenges 
      * Find value to do things in .NET Core rather than complete rewrite just because you can

## Questions

> What paths are there to migrate off of WCF or WebForms?
```
For WCF Move to APIs or RPC architectures.
For WebForms, transition to MVC or Razor Pages.

For both, extract all the business logic to libraries and get them behind automated testing. Then upgrade the interfaces to newer technologies.
```

> Is Kestrel production ready?
```
It is, though it's recommended to run behind a reverse proxy especially if you are wanting to distribute load across multiple ports on the same hardware or if your app is public facing.
```

> Are other DI solutions available in .NET Core?
```
Yes; AutoFac, StructureMap, and others are available in Core. 
```



## Workshop Pre-requisites

Sample is a MVC application.
* Windows machine or VM required for first part.
* Uses SQL Server database, SQL Server Express with local DB can be used.
* .NET Core 3
* .NET Framework 4.6.1
* Visual Studio 2017/19
* [.NET Portability Analyzer](https://docs.microsoft.com/en-us/dotnet/standard/analyzers/portability-analyzer)
* Docker for Windows
* Internet connection

Workshop GitHub [MovieMvc](https://github.com/seankilleen/netcoreworkshop-mvcmovie-sample)

Different stages of the workshop are split out into various branches.

## Workshop

### Setup

1. After downloading pre-requisites and setting up the solution, in VS Package Manager console run `Update-Database` to seed the database. 
1. All unit/integration tests should run after that.

### Modernizing

#### Preparing for migration - making everything consistent

1. Make all dependencies (NuGet packages) the same versions
1. Get Framework to version 4.7.2 (or latest)
   1. `Update-Package -Reinstall` to reinstall all packages for newer Framework version.
1. Build and make sure tests still pass.
1. Try to run the application (expect it to break, Google error messages to find fixes).

#### Preparing for migration - analyzing differences

1. Run Portability Analyzer
1. Install Microsoft.DotNet.Analyzers.Compatibility (preview)
   * Rerun build, not-supported code will show up as build messages and link you to where it exists
1. Identify frameworks you're currently utilizing which will need to be converted (DI, database, configurations, bundling, filtering, etc.)
   * A lot of this will be in Global.asax/Startup.cs
   * There is a lot of information on how to convert each of these independently available online.

#### Migrating

1. One of the biggest changes is the CSPROJ file structure.
   * Rather than attempt to update CSPROJ, create brand new set of projects.
1. Add known and necessary packages and project references
1. Move controllers and related views into new project and fix issues one set (controller/view pair) at a time.
1. Copy migrations.
1. Configure DI, other middleware.
1. Make sure configurations are set (DB connection string).
1. Run DB migrations: `Update-Database`
1. Migrate test files.

#### Choose your own adventure

* Health Checks
* Containerization
* Self-Hosting
* WebApi
* Identity and Auth

### Learnings

* When running unit/integration tests that rely on database setup, run the tests within the context of a tranasaction that can be discarded so that when the test ends, the related database setup/changes will be removed. This will keep tests from interfering with each other.
* .NET Core middleware in Startup.cs is added in the pipeline in the same order as you add them in code. Be mindful that you are not causing unexpected issues by adding middleware out of order.
* EntityFramework is now compatible with .NET Core (as of v6.3.0), instead of using Microsoft.EntityFrameworkCore, it is recommended to use plain 'ol EntityFramework (YEA!).


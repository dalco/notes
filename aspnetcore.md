Building a Web App with ASP.NET Core, MVC 6, EF Core and Angular
==================================================================

What is ASP.NET Core?
---------------------

### ASP.NET Core Hello World

1. Command Prompt -> `cd newprojectfolder`
2. `dotnet new`
3. `dotnet restore` (controls the dependencies in project.json and downdload from NuGet)
4. `dotnet run` (build and run app)

### Starting a new Web App without Visual Studio

1. Install Yeoman and aspnet generator globally
2. Run `yo aspnet` to start a new app
3. Select Web Application from the list
4. Select Bootstrap from the list
5. This create the structure and then install bower dependencies
6. `dotnet restore` to install NuGet packages
7. `dotnet run`
*to open the current project with Visual Studio Code `code .`

### ASP.NET Core Web App In Visual Studio

1. Install Web Essentials 2015
2. Install Web Compiler
3. Install Add new file
4. Install Typescript Tools
5. New Project -> Web -> ASP.NET Core Web Application (.NET Core)
6. Web Application || Empty

HTML and CSS Basics
---------------------

### Serving Files in ASP.NET 5
1. `public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    app.UseStaticFiles();
}` in Startup.cs (configure of the middleware)
2. Move the index.html file created in the wwwroot folder
3. `app.UseDeafultFiles();` before `app.UseStaticFiles();` in Startup

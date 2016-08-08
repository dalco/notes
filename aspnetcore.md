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
3. `app.UseDeafultFiles();` before `app.UseStaticFiles();` in Startup.cs

MVC 6
-----

### First controller/view
1.  Create the new _Controllers_ folder
2.  Create the new _AppController.cs_ MVC Controller Class file inside the folder.
3.  Add MVC to _project.json_
4.  Add _Microsoft.AspNetCore.Mvc_ namespace to _AppController.cs_
5.  Create the new _Views/App_ folder. The folder has to match the name of the Controller
6.  Create new _index.cshtml_ file inside the folder
7.  In _Startup.cs_ _Configure_ delete the `app.UseDefaultFiles()` method to stop serving index.html file from wwwroot folder
``
### Enabling MVC 6
1.  Add _app.UseMvc(); in _Startup.cs_ to enable MVC
2.  Add `services.AddMvc();` in _Startup.cs_ _ConfigureServices_ ASP.NET Core use dependency injection -> we are injecting the MVC service
3.  Add routes configuration `app.UseMvc(config =>
                                          {
                                              config.MapRoute(
                                                  name: "Default",
                                                  template: "{controller}/{action}/{id?}",
                                                  defaults: new { controller = "App", action = "Index" }
                                              );
                                          });`
                                                                      
### Creating a layout
1.  Under _Views_ create the _Shared_ folder
2.  In _Shared_ folder create a new _MVC view layout page_
3.  Copy the content of the _index.cshtml_ file and paste it into the new __Layout.cshtml_ file
4.  In the _index.cshtml_ file keep only the content of for the home page and cut the rest
5.  Cut the content of the _index.cshtml_ file from the __Layout.cshtml_ file
6.  Add `@RenderBody()` to __Layout.cshtml_ to render the body of the page
7.  Under _Views_ create a new _MVC view start page_ to specify the layout page

### Adding more views


 


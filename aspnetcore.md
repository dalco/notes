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
                                          });` e.g. {controller}/{action}/{id?} = app/about
                                                                      
### Creating a layout
1.  Under _Views_ create the _Shared_ folder
2.  In _Shared_ folder create a new _MVC view layout page_
3.  Copy the content of the _index.cshtml_ file and paste it into the new __Layout.cshtml_ file
4.  In the _index.cshtml_ file keep only the content of for the home page and cut the rest
5.  Cut the content of the _index.cshtml_ file from the __Layout.cshtml_ file
6.  Add `@RenderBody()` to __Layout.cshtml_ to render the body of the page
7.  Under _Views_ create a new _MVC view start page_ to specify the layout page

### Adding more views
1.  Add a new ` public IActionResult About()
                       {
                           return View();
                       }` in the _AppController.cs_
2. Add the _cshtml_ file in the _Views/App_ folder whose name match the one of the controller
3. Add _~/_ before references in order to replace with the actual root
4. Add `throw new InvalidOperationException("Bad things happen");` to the _contact_ action in _AppController_
5. Add `app.UseDeveloperExceptionPage();` to _Configure_ in _Startup_
6. Add `IHostingEnvironment` interface to _Configure_ in _Startup_
7. Add `if (env.IsEnvironment("Development"))
                    {
                        app.UseDeveloperExceptionPage();
                    }` to _Configure_ in _Startup_ in order to use the _app.UseDeveloperExceptionPage()_ only on _Development_ machine
                    
### Using tag helpers
1.  Replace _href_ with _asp-controller_ and _asp-action_ in __Layout.cshtml_
2.  Add _Microsoft.AspNetCore.Mvc.TagHelpers_ to _project.json_
3.  In _Views_ folder add a new _MVC View Imports Page_
4.  In the new __ViewImports.cshtml_ file add _@addTagHelper "*, Microsoft.AspNetCore.Mvc.TagHelpers"_ this is going to inject the tag helpers in every view

### Implementing a contact page
1.  Create a form in _Contact.cshtml_
2.  Create a new _ViewModels/ContactViewModel_ class with properties in the form
3.  Add _asp-for_ tag in the form in in _Contact.cshtml_
4.  Add `@model TheWorld.ViewModels.ContactViewModel` in _Contact.cshtml_

### Using validation
1.  Add `[Required]` to every property in _ContactViewModel.cs_
2.  Add _jquery-validation_ and _jquery-validation-unobtrusive_ to _bower.json_
3.  Add `@section scripts{
         <script src="~/lib/jquery-validation/dist/jquery.validate.min.js"></script>
         <script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"></script>
         }` in _Contact.cshtml_
4.  Add `@RenderSection("scripts", false);` after _jquery_ in __Layout.cshtml_ in order to call at this point the _section scripts_ of the point 3 after _jquery_ and only when in _contact_ page
5.  Add validation messages using `<span asp-validation-for=""></span>` in _Contact.cshtml_
6.  Add `<span asp-validation-summary="ModelOnly"></span>` at the top of the form in order to show messages that concern the entire form

### Supporting POST
1.  Add `method="post"` in the _form_ tag
2.  Add ` [HttpPost]
                 public IActionResult Contact(ContactViewModel model)
                 {
                     return View();
                 }` action in _AppController.cs_ (remember to add _using TheWorld.ViewModels;_ to use the _ContactViewModel_)
                 
### Adding a service
1.  Create a _Services_ folder in the project 
2.  Create a new _IMailService.cs_ interface file in the _Services_ folder
3.  Create the interface: ` public interface IMailService
         {
             void SendMail(string to, string from, string subject, string body);
         }`
4.  Create a new _DebugMailService.cs_ class file in the _Services_ folder
5.  Implement the _IMailService_ interface in the _DebugMailService.cs_ class
6.  Add the _IMailService_ to the _AppController.cs_ `private IMailService _mailService;
                                                              public AppController(IMailService mailService)
                                                              {
                                                                  _mailService = mailService;
                                                              }`
7.  Call the __mailService.SendMail_ method in the `[HttpPost]
                                                                     public IActionResult Contact(ContactViewModel model)`
8.  In _Startup.cs_ add `private IHostingEnvironment _env;
                                 public Startup(IHostingEnvironment env)
                                 {
                                     _env = env;
                                 }`
9.  In _ConfigureServices_ (_Startup.cs_) add `if (_env.IsEnvironment("Development") || _env.IsEnvironment("testing"))
                                                           {
                                                               services.AddScoped<IMailService, DebugMailService>();
                                                           }
                                                           else {
                                                               //Implement a real mail service
                                                           }`
 
### Completing the form
1.  Create a _config.json_ file in the project 
2.  Create the `{
                  "MailSettings": {
                    "ToAddress":  "federico.dalcolle@gmail.com"
                  }
                }` object in _config.json_
3.  In _Startup.cs_ add `var builder = new ConfigurationBuilder()
                .SetBasePath(_env.ContentRootPath)
                .AddJsonFile("config.json");
_config = builder.Build();` in _Startup_ and `services.AddSingleton(_config);` in _ConfigureServices_
4.  Inject `IConfigurationRoot config` in _AppController.cs_ and add `_config["MailSettings:ToAddress"]` as first parameter in _mailService.SendMail_
5.  Custom validation example: add `if (model.Email.Contains("aol.com")) {
                                                    ModelState.AddModelError("Email", "We don't support AOL addresses");
                                                }` in _public IActionResult Contact(ContactViewModel model)_ To send the error to the entire object just leave the _field_ parameter empty. E.g. `ModelState.AddModelError("", "We don't support AOL addresses");`
6.  To show a success message: add `if (ModelState.IsValid)
                                                {
                                                    _mailService.SendMail(_config["MailSettings:ToAddress"], model.Email, "From the world", model.Message);
                                                    ModelState.Clear();
                                                    ViewBag.UserMessage = "Message sent!";
                                                }` in _AppController.cs_ and `@if (ViewBag.UserMessage != null)
                                                                                  {
                                                                                      <div>@ViewBag.UserMessage</div>
                                                                                  }` in _Contact.cshtml_
                                                                                  
Entity Framework Core 1.0
-------------------------

### Creating entities
1.  Create a new _Models_ folder
2. In the _Models_ folder creare a new _Trip.cs_ file ` public class Trip
                                                           {
                                                               public int Id { get; set; }
                                                               public string Name { get; set; }
                                                               public DateTime DateCreated { get; set; }
                                                               public string UserName { get; set; }
                                                               public ICollection<Stop> Stops { get; set; }
                                                           }`
3.  And a new _Stop.cs_ file `public class Stop
                                  {
                                      public int Id { get; set; }
                                      public string Name { get; set; }
                                      public double Latitude { get; set; }
                                      public double Longitude { get; set; }
                                      public int Order { get; set; }
                                      public DateTime Arrival { get; set; }
                                  }`
                                  
### Creating the DbContext
1.  In the _Models_ folder create a new _WorldContext.cs_ file `public class WorldContext: DbContext
                                                                    {
                                                                        public WorldContext()
                                                                        {}
                                                                        public DbSet<Trip> Trips { get; set; }
                                                                        public DbSet<Stop> Stops { get; set; }
                                                                    }`
2.  In _Startup.cs_ _ConfigureServices_ add `services.AddDbContext<WorldContext>();` in order to register the Context Class

### Using the DbContext
1.  Inject the _WorldContext_ in the _AppController_
2.  Use the _WorldContext_ in the _Index()_ as follow: `public IActionResult Index()
                                                                {
                                                                    var data = _context.Trips.ToList();
                                                                    return View(data);
                                                                }` in order to ask db for data and show it in the view
3.  Add the override method to _WorldContext.cs_ `private IConfigurationRoot _config;
                                                          public WorldContext(IConfigurationRoot config)
                                                          {
                                                              _config = config;
                                                          }
                                                          protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
                                                                  {
                                                                      base.OnConfiguring(optionsBuilder);
                                                                      optionsBuilder.UseSqlServer(_config[""]);
                                                                  }`
4. To the _config.json_ add `"ConnectionStrings": {
                                 "WorldContextConnection":  "Server=(localdb)\\MSSQLLocalDb;Trusted_Connection=true;MultipleActiveResultSets=true"
                               }`
5.  Modify _WorldContext.cs_ to `optionsBuilder.UseSqlServer(_config["ConnectionStrings:WorldContextConnection"]);` and inject _DbContextOptions_

### Using Migrations
1.  Add ` "Microsoft.EntityFrameworkCore.Tools": {
               "version": "1.0.0-preview2-final",
               "type": "build"
             },` to _project.json_
2.  Open the Administrator console and digit the command _dotnet ef_
3.  Digit the command _dotnet ef migrations add InitialDatabase_
4.  Digit the command _dotnet ef database update_

### Seeding the database
1.  In _Models_ create the new class _WorldContextSeedData_
2. In _WorldContextSeedData_ add `private WorldContext _context;
                                          public WorldContextSeedData(WorldContext context)
                                          {
                                              _context = context;
                                          }
                                          public async Task EnsureSeedData() {
                                              if (!_context.Trips.Any())
                                              {
                                                  var usTrip = new Trip()
                                                  {
                                                      DateCreated = DateTime.UtcNow,
                                                      Name = "US trip",
                                                      UserName = "",
                                                      Stops = new List<Stop>()
                                                      {
                                                          new Stop() { Order = 1, Latitude = 27.700000, Longitude = 85.333333, Name = "Atlanta" } 
                                                      }
                                                  };
                                                     _context.Trips.Add(usTrip);
                                                  _context.Stops.AddRange(usTrip.Stops);
                                                  var worldTrip = new Trip()
                                                  {
                                                      DateCreated = DateTime.UtcNow,
                                                      Name = "World trip",
                                                      UserName = "",
                                                      Stops = new List<Stop>()
                                                      {
                                                           new Stop() { Order = 1, Latitude = 27.700000, Longitude = 85.333333, Name = "Atlanta" }
                                                      }
                                                  };
                                                  _context.Trips.Add(worldTrip);
                                                  _context.Stops.AddRange(worldTrip.Stops);
                                                  await _context.SaveChangesAsync();
                                              }
                                          }
                                      }`
                                      
3.  Add `services.AddTransient<WorldContextSeedData>();` in _Startup.cs_ _ConfigureServices_
4.  Inject _WorldContextSeedData seeder_ in _Configure_
5.  Add `seeder.EnsureSeedData.Wait();` in _Configure_


                                        
                                          

                 



 


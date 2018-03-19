# Getting Started with ASP.NET Core MVC #

In this article, we are going to create a really simple application using _ASP.NET Core MVC_ to get more familiar with this technology.

## Introduction ##

Through this post, we are going to build a really simple application using the _ASP.NET Core MVC_ technology. It is going to be pretty straightforward and this will allow us to have a brief overview of this tool. Here, we are going to use the second version of _ASP.NET Core_.

## What is ASP.NET Core? ##

_ASP.NET Core_ is an open-source software created by _Microsoft_. It allows us to develop web apps, services, IoT apps or mobile backends. The main difference with _ASP.NET_ is _ASP.NET Core_ is cross-platform. By the way, it lets us to run on _.NET Core_ or _.NET Framework_.

## What is MVC? ##

Firstly, let's refresh our mind and let's have a look at _MVC_. _MVC_, or _Model-View-Controller_, is a pattern that allows us to separate an application in three main components: _Model_, _View_, _Controller_. Now, we can debate over the question, "_is MVC a design pattern or an architectural pattern?_" but that's another subject. Here, we have to retain the following information:

### Model ###

The _Model_ represents the data business logic layer. The _Business Logic_ should be encapsulated in the model, along with any implementation logic for persisting the state of the application.

### View ###

_Views_ are responsible for presenting content through the user interface. There should be minimal logic within a _View_ and this logic should be related to presenting content.

### Controller ###

A Controller is a component that handles and responds to user input and interaction. It renders the appropriate _View_ with the _Model_ data as a response. So, it is responsible for selecting which _Model_ type to work with and which _View_ to render.

## Creating our project ##

Let's create our project, wherever we want, by doing the following commands:

    dotnet new mvc -o gameapplication
    cd gameapplication
<small>_Creating our project and entering the folder_</small>

We can also create our project inside _Visual Studio_ ("_New > Project > Visual C# > .NET Core > ASP.NET Core Web Application_", then select "_Web Application (Model-View-Controller)_"). This will generate a _Solution_ and the project itself.

## Project structure ##

Now that we created our application, let's take a look at the structure of our folder.

### Application folders ###

* _Controllers_ - _Controller_ classes
* _Models_ - _Model_ classes
* _Views_ - HTML files for views
* _wwwroot_ - static files like _CSS_ files, icons or images

### Configuration files ###

* _Program.cs_ - contains the "_Main()_" method and it is used to initiate, build, run the server and host the application
* _Startup.cs_ - used to configure middlewares on the request pipeline
* _bundleconfig.json_ - used to store the project bundling and minification configuration for scripts and styles

## First run ##

We can now run our project with the following command:

    dotnet run
<small>_Running our project_</small>

We can also hit "_F5"_, or click the "_play"_ button in _Visual Studio_ and wait a few seconds.

Our project will be available at "_http://localhost:RANDOM-PORT_".

## Adding the Entity Framework Core ##

For our project, we are going to use the _Entity Framework Core_. _Entity Framework Core_ is an _object-relational mapper_ (_ORM_) that allows us to work with a database using _.NET objects_. It supports a development paradigm called _Code First_. It lets us create _Model_ objects by writing simple classes (also known as _POCO_ classes, from "_plain-old CLR objects_"). We can have the database created on the fly from our classes.

We can install the _Entity Framework Core_ with the following command:

    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    dotnet add package Microsoft.EntityFrameworkCore.Design
    dotnet add package Microsoft.EntityFrameworkCore.Tools 
<small>_Installing the Entity Framework Core_</small>

We can also use the _NuGet package manager_ through _Visual Studio_ like so:

    Install-Package Microsoft.EntityFrameworkCore.SqlServer
    Install-Package Microsoft.EntityFrameworkCore.Design
    Install-Package Microsoft.EntityFrameworkCore.Tools 
<small>_Installing the Entity Framework Core_</small>

## Creating the Context ##

In our project, let's create a folder named "_Data_". In this folder, we can now create a file named "_GameContext_" and place the following code into that one:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.EntityFrameworkCore;
    using GameApplication._Models_;

    namespace GameApplication.Data
    {
        public class GameContext : DbContext
        {
            public GameContext(DbContextOptions<GameContext> options) : base(options)
            {
            }

            public DbSet<Game> Games { get; set; }
        }
    }
<small>_Data/GameContext.cs file_</small>

Here we create only one "_DbSet_" property. A "_DbSet_" corresponds to a database table and an _Entity_ corresponds to a row in the table.

Now, in our "_Startup.cs_" file, we have to add the following lines:

    ...
    using Microsoft.EntityFrameworkCore;
    ...
    using GameApplication.Data;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<GameContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
        services.AddMvc();
    }
<small>_Startup.cs file edited_</small>

Here, we register our context for _Dependency Injection_.

## Using LocalDB ##

_LocalDB_ is a lightweight version of the _SQL Server Express Database Engine_. _LocalDB_ runs in a special execution mode of _SQL Server Express_ that allows us to work with databases as "_.mdf_" files. _LocalDB_ is installed by default with _Visual Studio_.

To use _LocalDB_, we have to change the connection string. So, in our "_appsettings.json_" file, we have to add the following things:

    "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=GameApplication;Trusted_Connection=True;MultipleActiveResultSets=true"
    }
<small>_appsettings.json file edited_</small>

## Initializing Data ##

In our "_Data_" folder, let's create a file named "_DbInitializer_" and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using GameApplication._Models_;

    namespace GameApplication.Data
    {
        public class DbInitializer
        {
            public static void Initialize(GameContext context)
            {
                if (context.Games.Any())
                {
                    return;
                }

                var games = new List<Game>
                {
                    new Game{Name="Super Mario Bros"},
                    new Game{Name="The Legend of Zelda"},
                    new Game{Name="Metroid"},
                    new Game{Name="Donkey Kong Country"}
                };

                games.ForEach(g => context.Games.Add(g));
                context.SaveChanges();
            }
        }
    }
<small>_Data/DbInitializer.cs file_</small>

We now have to change the "_Main()_" method in the "_Program.cs_" file like so:

    ...
    using Microsoft.Extensions.DependencyInjection;
    ...
    using GameApplication.Data;

    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);

        using (var scope = host.Services.CreateScope())
        {
            var services = scope.ServiceProvider;
            try
            {
                var context = services.GetRequiredService<GameContext>();
                DbInitializer.Initialize(context);
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred while seeding the database.");
            }
        }

        host.Run();
    }
<small>_Program.cs filed edited_</small>

## Migrations ##

In a real application, our data model will change frequently. So, for each change, we need to sync that database. We could configure the _Entity Framework Core_ to automatically drop and re-create the database each time we change the data model. This is alright for development, but not for production because we would lose data. To solve this problem, we need to use the _Code First Migrations_ feature.

Let's create and execute a _Migration_ like so:

    dotnet ef migrations add InitialCreate
    dotnet ef database update
    // OR
    add-migration InitialCreate
    update-database
<small>_Creation and running a Migration_</small>

We can use the _Server Explorer_ to see that our migration has been run. Now, the first time we run the application, the database will be seeded with test data.

## Creating our Controller ##

Now, we are going to create our "_GamesController_". To achieve this, we are going to use the "_scaffold_" option, so we need _Visual Studio_ and it is required that our project is embedded in a _Solution_. So, we can right click on the "_Controllers_" folder and select " _MVC controller with views, using Entity Framework_". Let's name our _Controller_ "_Games_". For the _Model_ class, we can select "_Game_" and for the _Context_ "_GameContext_". We can also choose to use shared view "_\_Layout.cshtml_". After a few seconds, we can see that a _Controller_ with some code in it and a bunch of _Views_ have been created for us.

It is time to run our application again. Let's go to "_http://localhost:RANDOM-PORT/games_" to see the result. As we can see by looking at the result and the code, a lot has been made for us.

## Conclusion ##

Through this article, we got an overview of the _ASP.NET Core MVC Framework_. We saw the basic concepts it uses. We built a small application using the _MVC_ pattern and saw the different options that we have to create a dynamic web application. We saw how to create basic _Models_, _Views_ and _Controllers_. We had a brief overview of the _Entity Framework_ and how to use it. We are now ready to get further, because, of course, here, we didn't talk about production environment or other elements.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
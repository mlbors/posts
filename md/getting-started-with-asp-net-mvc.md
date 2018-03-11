# Getting started with ASP.NET MVC #

In this article, we are going to create a really simple application using _ASP.NET MVC_ to get more familiar with this technology.

## Introduction ##

Through this post, we are going to build a really simple application using the _ASP.NET MVC_ technology. It is going to be pretty straightforward and this will allow us to have a brief overview of this tool.

## What is ASP.NET MVC? ##

_ASP.NET MVC_ is an open-source software created by _Microsoft_. In a few words, it is some kind of web development framework based on _ASP.NET_. As its name tells us, it is an _MVC_ framework and this last one allows us to create dynamic web applications.

Recently, _Microsoft_ released _ASP.NET Core_. This last one also allows us to develop applications using the _MVC_ pattern, but has a slightly different approach. The main differences are that _ASP.NET Core_ is open source and cross-platform. But we are not going to focus on this one here.

## What is MVC? ##

Firstly, let's refresh our mind and let's have a look at _MVC_. _MVC_, or _Model-View-Controller_, is a pattern that allows us to separate an application in three main components: _Model_, _View_, _Controller_. Now, we can debate over the question, "_is MVC a design pattern or an architectural pattern?_" but that's another subject. Here, we have to retain the following information:

### Model ###

The _Model_ represents the data business logic layer. The _Business Logic_ should be encapsulated in the model, along with any implementation logic for persisting the state of the application.

### View ###

_Views_ are responsible for presenting content through the user interface. There should be minimal logic within a _View_ and this logic should be related to presenting content.

### Controller ###

A _Controller_ is a component that handles and responds to user input and interaction. It renders the appropriate _View_ with the _Model_ data as a response. So, it is responsible for selecting which _Model_ type to work with and which _View_ to render.

## Creating our project ##

Let's head to _Visual Studio_ and choose "_New Project > Visual C# > Web > ASP.NET We Application (.NET Framework)_". After we entered a name for our application, we can go to the next step and chose the "_MVC_" option. We can check the box for unit tests if we want to generate a second assembly for unit testing.

## Project structure ##

Now that our application is generated, let's take a look at the different folders we have.

### Application information ###

* Properties - application properties (application type, startup object, assembly information)
* References - used to pull additional libraries into the project

### Application folders ###

* App_Data - application data (for example, SQL database)
* Content - static files like CSS files, icons or images
* Controllers - Controller classes
* Fonts - custom font files
* Models - Model classes
* Scripts - JavaScript files
* Views - HTML files for views

### Configuration files ###

* Global.asax - contains code for responding to application-level events raised by ASP.NET or by HttpModules
* packages.config - is managed by the NuGet infrastructure and tracks installed packages with their respective versions
* Web.config - contains settings that apply to the web site

## First run ##

Now, let's hit "_F5_", or click the "_play_" button in _Visual Studio_ and wait a few seconds. This will start _IIS Express_ and run our web app. _Visual Studio_ then launches a browser and opens the application's home page. If everything is alright, our application will by available at "_localhost:RANDOM_PORT_".

## Creating a Controller, a Model and a View ##

It is now time to create our first _Controller_. To achieve this, in the _Solution Explorer_, we need to right-click on the _Controllers_ folders and choose "_Add > Controller_". Select "_MVC 5 Controller - Empty_" and name our _Controller_ "_GamesController_". As we can see, the _Controller_ was created in the "_Controllers_" folder, but a new folder, named "_Games_" was also generated in the "_Views_" folder. Now, let's create a _Model_ by right-clicking on the _Models_ folder and choosing "_Add > Class_". Let's call this Model "_Game_". Let's give our _Model_ a few properties.

    public class Game
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
<small>_Models/Game.cs file_</small>

Now let's create a _View_. For this, we need to right-click on the "_Views/Games_" folder and select "_Add > View_". Let's name this view "_Index_" and, under the "_Layout_" option, select the layout named "_\_Layout.cshtml_" in "_Views/Shared_".

Let's go back to our "_GamesController_" and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using WebApplication1.Models;

    namespace WebApplication1.Controllers
    {
        public class GamesController : Controller
        {
            // GET: Games
            public ActionResult Index()
            {
                Game game = new Game() { Name = "Super Mario Bros" };
                return View(game);
            }
        }
    }
<small>_Controllers/GamesController.cs file_</small>

Here, we just have one action, "_Index()_", where we create a new "_Game_" object before passing it to the specific _View_. the "_View()_" method is inherited from the base _Controller_ class. It returns a "_ViewResult_" object that is derived from "_ActionResult_".

In our recently created _View_, let's put the following code:

    model WebApplication1.Models.Game
    @{
        ViewBag.Title = "Index";
        Layout = "~/Views/Shared/_Layout.cshtml";
    }

    <h2>@Model.Name</h2>
<small>_Views/Games/Index.cshtml file_</small>

At the top of the file, we can see a few properties. The "_@model_" at the very top defines the type of the _Model_ used with this _View_. "_ViewBag.Title_" is basically the title of the page showed in the browser. "_Layout_" is the layout for this view. In the body of the _View_, we can see something like "_@Model_". Every View has this property which gives us access to the _Model_ we pass to it in the _Controller_.

Now, if we go to "_localhost:RANDOM_PORT/games_", we should have something interesting.

## Creating Routes & Action Parameters ##

Now it is time to confront ourselves with _Routes_ and _Action Parameters_.

When a request comes into our application, the _ASP.NET MVC Framework_ maps request data to parameters values for _Action Methods_. If an _Action Method_ takes a parameter, the framework looks for a parameter with the exact same name in the request data. If the parameter exists, it will be passed to the _Action_.

Let's define a new _Action_ in our "_GamesController_":

    public ActionResult Edit(int id, string stringParameter, int? optionalParameter)
    {
        if (String.IsNullOrWhiteSpace(stringParameter))
        {
            stringParameter = "None";
        }

        if (!optionalParameter.HasValue)
        {
            optionalParameter = 1;
        }

        return Content(String.Format("id={0}&stringParameter={1}&optionParameter={2}", id, stringParameter, optionalParameter));
    }
<small>_Controllers/GamesController.cs file edited_</small>

Here, we add three parameters to our _Action_. The first is and _ID_, the second a _string_ and the third an _optional integer_. Now if we go to "_localhost:RANDOM_PORT/games/edit?id=1&stringParameter=someString&optionalParameter=42_", we should have something on the screen. Here, we just use the query string to pass our parameters. But we can do something more elegant by defining some Routes.

_Routes_ are available in the "_App_Start/RouteConfig.cs_" file. As we can see, there is a default _Route_ that works for most scenarios. However, let's define a custom _Route_. Let's place the following code before the default _Route_.

    routes.MapRoute(
        name: "EditGame",
        url: "games/edit/{id}/{stringParameter}/{optionalParameter}",
        defaults: new { controller = "Games", action = "Edit" }
    );
<small>_Defining a custom Route in App_Start/RouteConfig.cs_</small>

If we go to "_localhost:RANDOM_PORT/games/edit/1/foo/42_", we should see something.

This way to create _Routes_ is not really clean if we have many custom _Routes_. So, let's delete this code and replace it with this one:

    routes.MapMvcAttributeRoutes();
<small>_Adding Attributes Routes in App_Start/RouteConfig.cs_</small>

Now, go back to our "_GamesController_" to edit it like so:

    [Route("games/edit/{id}/{stringParameter}/{optionalParameter:regex(\\d{2})}")]
    public ActionResult Edit(int id, string stringParameter, int? optionalParameter)
    {
        ...
    }
<small>_Controllers/GamesController.cs file edited_</small>

Here, we just place our URL pattern in an attribute before our "_Edit()_" action. Notice that we also, on this occasion, add a constraint to the third parameter.

## View Models ##

For now, we just display the name of a game in our view "_Index_". What if we want to display more information? To achieve this, we can use a _View Model_. A _View Model_ is a _Model_ build for a _View_, including data and rules specific to that _View_.

Let's say we want to display characters that are in our game. To achieve this, let's create a new _Model_ name "_Character_".

    public class Character
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
<small>_Models/Character.cs file_</small>

Now, let's create a folder named "_ViewModels_" at the root of our project. We can now place a new class in this folder. Name this last one "_IndexGameViewModel_". We can fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using WebApplication1.Models;

    namespace WebApplication1.ViewModels
    {
        public class IndexGameViewModel
        {
            public Game Game { get; set; }
            public List<Character> Characters { get; set; }
        }
    }
<small>_ViewModels/IndexGameViewModel.cs file_</small>

Now, go back to our "_GamesController_" and edit it like so:

    using System.Web;
    using System.Web.Mvc;
    using WebApplication1.Models;
    using WebApplication1.ViewModels;

    namespace WebApplication1.Controllers
    {
        public class GamesController : Controller
        {
            // GET: Games
            public ActionResult Index()
            {
                Game game = new Game() { Name = "Super Mario Bros" };
                List<Character> characters = new List<Character>
                {
                    new Character { Name = "Mario" },
                    new Character { Name = "Luigi" },
                    new Character { Name = "Peach" },
                    new Character { Name = "Bowser" }
                };

                IndexGameViewModel viewModel = new IndexGameViewModel()
                {
                    Game = game,
                    Characters = characters
                };

                return View(viewModel);
            }

            ...
        }
    }
<small>_Controllers/GamesController.cs file edited_</small>

As we can see, we added a few lines in our "_Index()_" action. We created a "_List_" containing a list of characters and a "_IndexGameViewModel_" object that we pass to the _View_.

We also have to edit our _View_ like so:

    @using WebApplication1.Models;

    @model WebApplication1.ViewModels.IndexGameViewModel
    @{
        ViewBag.Title = "Index";
        Layout = "~/Views/Shared/_Layout.cshtml";
    }

    <h2>@Model.Game.Name</h2>

    <ul>
        @foreach (Character character in Model.Characters)
        {
            <li>@character.Name</li>
        }
    </ul>
<small>_Views/Games/Index.cshtml file edited_</small>

We can go back to "_localhost:RANDOM_PORT/games_" to see the result.

## Using Entity Framework ##

Let's go a little further and use the _Entity Framework_. _Entity Framework_ is an _object-relational mapper_ (_ORM_) that allows us to work with a database using _.NET_ objects. It supports a development paradigm called _Code First_. It lets us create _Model_ objects by writing simple classes (also known as _POCO_ classes, from "_plain-old CLR objects_"). We can have the database created on the fly from our classes.

First, we need to install the _Entity Framework_. We can achieve this by calling the graphic _Package Manager_ or using the command line version like so:

    PM> Install-Package EntityFramework -Version 6.2.0 
<small>_Installing Entity Framework_</small>

Now, let's create another _Model_ named "_Enemy_" and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Web;

    namespace WebApplication1.Models
    {
        public class Enemy
        {
            public int Id { get; set; }
            public string Name { get; set; }
        }
    }
<small>_Models/Enemy.cs file_</small>

Now, we can create a new folder, at the root of our project, named "_DAL_" (_Data Access Layer_). We now need to create a "_Context_". It is the class that coordinates _Entity Framework_ functionality for a given data model.

    using System;
    using System.Data.Entity;
    using System.Data.Entity.ModelConfiguration.Conventions;
    using WebApplication1.Models;

    namespace WebApplication1.DAL
    {
        public class GameContext : DbContext
        {

            public GameContext() : base("GameContext")
            {
            }

            public DbSet<Enemy> Enemies { get; set; }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
            }
        }
    }
<small>_DAL/GameContext.cs file_</small>

First, we pass the name of the connection string to the _constructor_.

Then, and because it is just a really simple example, we create only one "_DbSet_" property. A "_DbSet_" corresponds to a database table and an entity corresponds to a row in the table.

Finally, we specify we don't want our tables in the database to be pluralized.

We can now initialize the database with test data. So, in the "_DAL_" folder, we can create a file named "_GameInitializer_".

    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Web;
    using WebApplication1.Models;

    namespace WebApplication1.DAL
    {
        public class GameInitializer : System.Data.Entity.DropCreateDatabaseIfModelChanges<GameContext>
        {
            protected override void Seed(GameContext context)
            {
                var enemies = new List<Enemy>
                {
                    new Enemy{Name="Goomba"},
                    new Enemy{Name="Koopa Troopa"},
                    new Enemy{Name="Lakitu"},
                    new Enemy{Name="Boo"}
                };

                enemies.ForEach(e => context.Enemies.Add(e));
                context.SaveChanges();
            }
        }
    }
<small>_DAL/GameInitializer.cs file_</small>

Here, we initialize the database with test data. We specify that we want to drop the database each time the model changes. This is great for development, but obviously, we don't want it in production.

The "_Seed()_" method takes the database context object as an input parameter, and the code in the method uses that object to add new entities to the database. 

We now have to edit the "_Web.config_" file to use our initializer.

    <entityFramework>
        <contexts>
            <context type="WebApplication1.DAL.GameContext, WebApplication1">
                <databaseInitializer type="WebApplication1.DAL.GameInitializer, WebApplication1" />
            </context>
        </contexts>
        ...
    </entityFramework>
<small>_Web.config file edited_</small>

## Setting up Entity Framework to use LocalDB ##

_LocalDB_ is a lightweight version of the _SQL Server Express Database Engine_. _LocalDB_ runs in a special execution mode of _SQL Server Express_ that allows us to work with databases as "_.mdf_" files. _LocalDB_ is installed by default with _Visual Studio_.

Let's edit again our "_Web.config_" file:

    <connectionStrings>
        <add name="GameContext" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=WebApplication1;Integrated Security=SSPI;" providerName="System.Data.SqlClient"/>
    </connectionStrings>
    <appSettings>
        ...
    </appSettings>
<small>_Web.config file edited_</small>

## Another Controller ##

We are now going to create another _Controller_ name "_EnemyController_" using the "_MVC 5 Controller with views, using Entity Framework_" option. We can enter "_Enemy_" as the _Model_ class and "_GameContext_" for the database context. After a few seconds, we can see that a _Controller_ with some code in it and a bunch of _Views_ have been created for us.

It is time to run our application again. Let's go to "_localhost:RANDOM_PORT/enemy_" to see the result. As we can see by looking at the result and the code, a lot has been made for us.

## Migrations ##

In a real application, our data model will change frequently. So, for each change, we need to sync that database. For now, we have configured the _Entity Framework_ to automatically drop and re-create the database each time we change the data model. This is alright for development, but not for production because we would lose data. To solve this problem, we need to enable the _Code First Migrations_ feature.

First, we need to edit the "_Web.config_" file:

    ...
    <connectionStrings>
        <add name="GameContext" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=WebApplication12;Integrated Security=SSPI;" providerName="System.Data.SqlClient"/>
    </connectionStrings>
    ...
    <!-- remove or comment <contexts>
        <context type="WebApplication1.DAL.GameContext, WebApplication1">
            <databaseInitializer type="WebApplication1.DAL.GameInitializer, WebApplication1" />
        </context>
    </contexts> -->
<small>_Web.config file edited_</small>

It isn't required to change the database as we did here, but that will let the first migration create a new database.

We can now enable migrations:

    PM> enable-migrations
    PM> add-migration InitialCreate
<small>_Enabling migrations and adding a migration_</small>

As we can see, a "_Migrations_" folder has been created. In this folder, we can find a file "_Configuration_" that contains a "_Seed()_" method like our initializer. Let's place the following code into that file:

    protected override void Seed(WebApplication1.DAL.GameContext context)
    {
        var enemies = new List<Enemy>
        {
            new Enemy{Name="Goomba"},
            new Enemy{Name="Koopa Troopa"},
            new Enemy{Name="Lakitu"},
            new Enemy{Name="Boo"}
        };

        enemies.ForEach(e => context.Enemies.AddOrUpdate(p => p.Name, e));
        context.SaveChanges();
    }
<small>_Migrations/Configuration.cs file_</small>

In the "_Migrations_" folder, we can also find the code that would create the database from scratch. The "_Up()_" method creates the database tables that correspond to the data model entity sets, and the "_Down()_" method deletes them.

Now, edit our "_Enemy_" _Model_ like so:

    public class Enemy
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Level { get; set; }
    }
<small>_Models/Enemy.cs file edited_</small>

Then, change the "_Configuration_" file:

    protected override void Seed(WebApplication1.DAL.GameContext context)
    {
        var enemies = new List<Enemy>
        {
            new Enemy{Name="Goomba", Level=1},
            new Enemy{Name="Koopa Troopa", Level=2},
            new Enemy{Name="Lakitu", Level=5},
            new Enemy{Name="Boo", Level=4}
        };

        enemies.ForEach(e => context.Enemies.AddOrUpdate(p => p.Name, e));
        context.SaveChanges();
    }
<small>_Migrations/Configuration.cs file edited_</small>

We can now create a migration and execute it:

    PM> add-migration EnemyModelEdited
    PM> update-database -TargetMigration EnemyModelEdited
<small>_Creating and executing a migration_</small>

We can use the _Server Explorer_ to see that our migration has been run.

## Unit Testing ##

Let's talk a little bit about unit tests. If we checked the appropriate box when we created the project, we should see, in the _Solution Explorer_, a second _Assembly_ made for unit tests. If it isn't the case, we have to add a new project to our solution (right-click on our solution, "_Add > Project_") using the unit test template. After that, we had to create a reference to our first project in the second project (right-click on "_References_", "_Add reference_"). We also need to add a few other things like "_System.Web_" in reference. We then need to install the "_Microsoft AspNet Mvc_" package. This will probably require from us to update the package in the first project.

Keep things very simple and in the file "_UnitTest1.cs_", put the following code:

    using System;
    using System.Web;
    using System.Web.Mvc;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using WebApplication1;
    using WebApplication1.Controllers;

    namespace WebApplication1Test
    {
        [TestClass]
        public class GameControllerTest
        {
            [TestMethod]
            public void Index()
            {
                GamesController controller = new GamesController();
                ViewResult result = controller.Index() as ViewResult;
                Assert.IsNotNull(result);
            }
        }
    }
<small>_UnitTest1.cs file_</small>

We can now execute our test and if everything is alright, we should have a success message.

## Conclusion ##

Through this article, we got an overview of the _ASP.NET MVC Framework_. We saw the basic concepts it uses. We built a small application using the _MVC_ pattern and saw the different options that we have to create a dynamic web application. We saw how to create basic _Models_, _Views_ and _Controllers_. We had a brief overview of the _Entity Framework_ and how to use it. We are now ready to get further, because, of course, here, we didn't talk about production environment or the _Repository Pattern_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!

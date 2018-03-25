# Connecting to SQL Server #

Through this article, we are going to see how we can install _SQL Server_ and connect an application to this last one.

## Introduction ##

_Microsoft SQL Server_ is a relational database management system developed by _Microsoft_. Here, we are going to have a look at how we can install it and how an _ASP.NET MVC_ application using the _Entity Framework_ can connect to this last one.

## Installing SQL Server ##

Head to [https://www.microsoft.com/en-us/sql-server/sql-server-downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads). Here, we are going to choose the _Developer Edition_. When the download is achieved, we just have to install it. So double-click on the installer and then choose the basic installation.

We may encounter a problem during the installation giving us the error _1638_. There are two solutions: we can uninstall _Visual Studio 2017_ completely, install _SQL Server_ then reinstall _Visual Studio 2017_ or uninstall _Microsoft Visual C++ 2017 Redistributable (x86) and (x64)_, install _SQL Server_ then reinstall _Microsoft Visual C++ 2017 Redistributable (x86) and (x64)_.

At the end of the installation, notice that several details are given, like the _Connection String_. We also have the opportunity to directly install SQL _Server Management Studio (SSMS)_. Here, we are going to do this also.

## Setting up the project ##

For our example, we are going to do a really simple application. Let's head to Visual Studio and choose "_New Project > Visual C# > Web > ASP.NET We Application (.NET Framework)_". After we entered a name for our application, we can go to the next step and chose the "_MVC_" option. We can check the box for unit tests if we want to generate a second assembly for unit testing.

Let's create a _Model_, named "_Movie.cs_" and fill it like so:

    public class Movie
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
<small>_Models/Movies.cs file_</small>

We can now create a folder named "_DAL_" and place a file named "_MovieContext.cs_" into it. Let's insert the following code inside that file:

    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.ModelConfiguration.Conventions;
    using System.Linq;
    using System.Web;
    using SQLServerApp.Models;

    namespace SQLServerApp.DAL
    {
        public class MovieContext : DbContext
        {
            public MovieContext() : base("MovieContext")
            {
            }

            public DbSet<Movie> Movies { get; set; }
        }
    }
<small>_DAL/MovieContext.cs_</small>

## Entity Framework and Migrations ##

We can now install the _Entity Framework_ like so:

    PM> Install-Package EntityFramework -Version 6.2.0
<small>_Installing the Entity Framework_</small>

We also have to set up our _Connection String_ in the "_Web.config_" file:

    <connectionStrings>
        <add name="MovieContext" connectionString="Data Source=OUR_SERVER\INSTANCE;Initial Catalog=MoviesDB;Integrated Security=True;MultipleActiveResultSets=True" providerName="System.Data.SqlClient"/>
    </connectionStrings>
<small>_Web.config file edited_</small>

Here, to make things work, we have to target the right server and the right instance.

We can now enable _Migrations_:

    PM> enable-migrations
<small>_Enabling migrations_</small>

In the newly created "_Migrations_" folder, we can find a file named "_Configuration.cs_". Just for our example, let's place the following code into the "_Seed()_" method:

    ...
    using System.Collections.Generic;
    ...
    using SQLServerApp.Models;

    protected override void Seed(SQLServerApp.DAL.MovieContext context)
    {
        var movies = new List<Movie>
        {
            new Movie{Name="2001: A Space Odyssey"},
            new Movie{Name="Gattaca"},
            new Movie{Name="Interstellar"},
            new Movie{Name="Cloud Atlas"}
        };

        movies.ForEach(e => context.Movies.AddOrUpdate(p => p.Name, e));
        context.SaveChanges();
    }
<small>_Migrations/Configuration.cs_</small>

Let's now create a _Migration_ and update the database:

    PM> add-migration Init
    PM> update-database
<small>_Creating a Migration and updating the database_</small>

## Check ##

We can use the _SQL Server Object Explorer_ to check if everything is right. We first have to add a server. Let's click on "_Add SQL Server_" and choose the targeted one. We can now see our database and our data. We can achieve the same thing through _SQL Server Management Studio_ by right-clicking our database's tables.

## Conclusion ##

Through this brief article, we saw how to install _SQL Server_ and how we can link an _ASP.NET MVC_ application using the _Entity Framework_ to this relational database management system.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!

# Using the Repository Pattern with the Entity Framework #

Through this article, we are going to see how to use the _Repository Pattern_ with the _Entity Framework_ in an _ASP.NET MVC_ application.

## Introduction ##

The _Repository Pattern_, as well as the _Unit of Work Pattern_, allows to create an abstraction layer between the data access layer and the business logic layer of an application. The purpose of creating this layer is to isolate data access layer so that the changes we may operate cannot affect the business logic layer directly. Implementing these patterns is also helpful for automated unit testing or test-driven development.

## The Repository Pattern ##

The _Repository Pattern_ allows us to create an abstraction layer between the data access layer and the business logic layer of an application. So, this _Data Access Pattern_ offers a more loosely coupled approach to data access. So, we are able to create the data access logic in a separate class, called a _Repository_, which has the responsibility of persisting the application's business model.

## The Unit of Work Pattern ##

The _Unit of Work Pattern_ is a pattern that handles the transactions during data manipulation using the _Repository Pattern_. _Unit of Work_ is referred to as a single transaction that involves multiple operations.

## Setting up our project ##

First, we have to set up our project. So, let's head to _Visual Studio_ and choose "_New Project > Visual C# > Web > ASP.NET We Application (.NET Framework)_". Name our application "_GameApplication_".

Now, let's install the _Entity Framework_ and enable _Migrations_:

    PM> Install-Package EntityFramework -Version 6.2.0 
    PM> enable-migrations
<small>_Installing Entity Framework and enabling migrations_</small>

Enable _Routes_ attributes:

    routes.MapMvcAttributeRoutes();
<small>_Adding Attributes Routes in App_Start/RouteConfig.cs file_</small>

We can now create our first _Model_, that we are going to name "_Game_", like so:

    namespace GameApplication.Models
    {
        public class Game
        {
            public int Id { get; set; }
            public string Name { get; set; }
        }
    }
<small>_Models/Game.cs file_</small>

We now have to create a "_DAL_" folder at the root of our application where we are going to create a "_GameContext_" file.

    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.ModelConfiguration.Conventions;
    using System.Linq;
    using System.Web;
    using GameApplication.Models;

    namespace GameApplication.DAL
    {
        public class GameContext : DbContext
        {
            public GameContext() : base("GameContext")
            {
            }

            public DbSet<Game> Games { get; set; }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
            }
        }
    }
<small>_DAL/GameContext.cs file_</small>

Now, in the "_Migrations/Configuration.cs_" file, let's place the following code:

    namespace GameApplication.Migrations
    {
        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.Migrations;
        using System.Linq;
        using GameApplication.Models;

        internal sealed class Configuration : DbMigrationsConfiguration<GameApplication.DAL.GameContext>
        {
            public Configuration()
            {
                AutomaticMigrationsEnabled = false;
            }

            protected override void Seed(GameApplication.DAL.GameContext context)
            {
                var games = new List<Game>
                {
                    new Game{Name="Super Mario Bros"},
                    new Game{Name="Super Mario 64"},
                    new Game{Name="Super Mario Galaxy"}
                };

                games.ForEach(e => context.Games.AddOrUpdate(p => p.Name, e));
                context.SaveChanges();
            }
        }
    }
<small>_Migrations/Configuration.cs file_</small>

We then have to make little changes in our "_Web.config_" file:

    <connectionStrings>
        <add name="GameContext" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=GameApplication;Integrated Security=SSPI;" providerName="System.Data.SqlClient"/>
    </connectionStrings>
<small>_Web.config file edited_</small>

Now, we are ready to create our first _Controller_. Let's choose the "_MVC 5 Controller with views, using Entity Framework_" option and name it "_GameController_". We can enter "_Game_" as the _Model_ class and "_GameContext_" for the database _Context_. After a few seconds, we can see that a _Controller_ with some code in it and a bunch of _Views_ have been created for us.

Now, if we run our application and go to "_localhost:RANDOM_PORT/game_", we should see something interesting.

## Generic Repository ##

There are many ways to implement the _Repository Patterns_. We could create a _Repository Class_ for each entity type, but it results in a lot of redundant code or in partial updates. So, to avoid this, we are going to create a _Generic Repository_.

First, let's create a file named "_GenericRepository.cs_" in our "_DAL_" folder and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Data.Entity;
    using System.Linq;
    using System.Linq.Expressions;
    using System.Web;
    using GameApplication.Models;

    namespace GameApplication.DAL
    {
        public class GenericRepository<TEntity> where TEntity : class
        {
            internal GameContext context;
            internal DbSet<TEntity> dbSet;

            public GenericRepository(GameContext context)
            {
                this.context = context;
                this.dbSet = context.Set<TEntity>();
            }

            public virtual IEnumerable<TEntity> Get(
                Expression<Func<TEntity, bool>> filter = null,
                Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy = null,
                string includeProperties = "")
            {
                IQueryable<TEntity> query = dbSet;

                if (filter != null)
                {
                    query = query.Where(filter);
                }

                foreach (var includeProperty in includeProperties.Split
                    (new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries))
                {
                    query = query.Include(includeProperty);
                }

                if (orderBy != null)
                {
                    return orderBy(query).ToList();
                }
                else
                {
                    return query.ToList();
                }
            }

            public virtual TEntity GetByID(object id)
            {
                return dbSet.Find(id);
            }

            public virtual void Insert(TEntity entity)
            {
                dbSet.Add(entity);
            }

            public virtual void Delete(object id)
            {
                TEntity entityToDelete = dbSet.Find(id);
                Delete(entityToDelete);
            }

            public virtual void Delete(TEntity entityToDelete)
            {
                if (context.Entry(entityToDelete).State == EntityState.Detached)
                {
                    dbSet.Attach(entityToDelete);
                }
                dbSet.Remove(entityToDelete);
            }

            public virtual void Update(TEntity entityToUpdate)
            {
                dbSet.Attach(entityToUpdate);
                context.Entry(entityToUpdate).State = EntityState.Modified;
            }
        }
    }
<small>_DAL/GenericRepository.cs file_</small>

As we can see, first, we declare two class variables: one for the context and one for the entity set that the _Repository_ is instantiated for. The _constructor_ accepts a database context instance and initializes the entity set variable.

If we just overview the class, we can notice that the code declares a typical set of "_CRUD_" methods.

The signature of the "_Get()_" method can seem impressive. The first two arguments are _lambda expressions_. "_Expression<Func<TEntity, bool>> filter_" means that a _lambda expression_, based on "_TEntity_", will be provided and it will return a Boolean value. "_Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy_" also means that a _lambda expression_ will be provided. The input of this expression is an "_IQueryable_" object and it will return an ordered version of that object. Finally, the third argument is a string that allows us to provide a comma-delimited list of navigation properties for eager loading. In the body of this method, we can see that, first, the filter expression is applied if there is one. Secondly, the eager-loading expression is performed. Finally, the order expression is applied if there is one.

Come next two methods, one to get an _Entity_ by its _ID_, then a method that handles insertion. We then have two "_Delete()_" methods that manage the deletion. Finally, we find the "_Update()_" method.

## Unit of Work ##

As we said, the role of the _Unit of Work_ class is to make sure that, when we use multiple _Repositories_, they share a single database _Context_. So, when a _Unit of Work_ is complete, the "_SaveChanges()_" method is called on the instance corresponding to the current _Context_. We are then assured that all related changes will be coordinated.

Let's create a file named "_UnitOfWork.cs_" in our "_DAL_" folder and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using GameApplication.Models;

    namespace GameApplication.DAL
    {
        public class UnitOfWork : IDisposable
        {
            private GameContext context = new GameContext();
            private GenericRepository<Game> gameRepository;

            public GenericRepository<Game> GameRepository
            {
                get
                {
                    return this.gameRepository ?? new GenericRepository<Game>(context);
                }
            }

            public void Save()
            {
                context.SaveChanges();
            }

            private bool disposed = false;

            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        context.Dispose();
                    }
                }
                this.disposed = true;
            }

            public void Dispose()
            {
                Dispose(true);
                GC.SuppressFinalize(this);
            }
        }
    }
<small>_DAL/UnitOfWork.cs file_</small>

If we look at the code, we can see that the class implements the "_IDisposable_" interface. The main purpose of this interface is to release unmanaged resources. A managed resource means "_managed memory_" that is managed by the garbage collector. When we no longer have any references to a managed object, which uses managed memory, the garbage collector will release that memory for us. So, unmanaged resources are everything that the garbage collector does not know about (open files, open network connections, etc.). So, using the "_Dispose()_" method of this interface allows us to release unmanaged resources in conjunction with the garbage collector. The consumer of an object can call this method when the object is no longer needed.

We also have a class variable for the database _Context_ and another for the _Repository_ we are going to use. If there were more _Repositories_, we would have to add variables representing each of them. We then perform a check to see if our _Repository_ already exists. If not, we instantiate it using the _Context_ instance.

The "_Save()_" method calls the "_SaveChanges()_" on the _Context_.

## Changing the Controller ##

Now, let's go back to our "_GameController_" and change it like so:

    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Data.Entity;
    using System.Linq;
    using System.Net;
    using System.Web;
    using System.Web.Mvc;
    using GameApplication.DAL;
    using GameApplication.Models;

    namespace GameApplication.Controllers
    {
        public class GameController : Controller
        {
            private UnitOfWork unitOfWork = new UnitOfWork();

            // GET: Game
            public ActionResult Index()
            {
                var games = unitOfWork.GameRepository.Get();
                return View(games.ToList());
            }

            // GET: Game/Details/5
            public ActionResult Details(int? id)
            {
                if (id == null)
                {
                    return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
                }
                Game game = unitOfWork.GameRepository.GetByID(id);
                if (game == null)
                {
                    return HttpNotFound();
                }
                return View(game);
            }

            // GET: Game/Create
            public ActionResult Create()
            {
                return View();
            }

            // POST: Game/Create
            [HttpPost]
            [ValidateAntiForgeryToken]
            public ActionResult Create([Bind(Include = "Id,Name")] Game game)
            {
                if (ModelState.IsValid)
                {
                    unitOfWork.GameRepository.Insert(game);
                    unitOfWork.Save();
                    return RedirectToAction("Index");
                }

                return View(game);
            }

            // GET: Game/Edit/5
            public ActionResult Edit(int? id)
            {
                if (id == null)
                {
                    return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
                }
                Game game = unitOfWork.GameRepository.GetByID(id);
                if (game == null)
                {
                    return HttpNotFound();
                }
                return View(game);
            }

            // POST: Game/Edit/5
            [HttpPost]
            [ValidateAntiForgeryToken]
            public ActionResult Edit([Bind(Include = "Id,Name")] Game game)
            {
                if (ModelState.IsValid)
                {
                    unitOfWork.GameRepository.Update(game);
                    unitOfWork.Save();
                    return RedirectToAction("Index");
                }
                return View(game);
            }

            // GET: Game/Delete/5
            public ActionResult Delete(int? id)
            {
                if (id == null)
                {
                    return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
                }
                Game game = unitOfWork.GameRepository.GetByID(id);
                if (game == null)
                {
                    return HttpNotFound();
                }
                return View(game);
            }

            // POST: Game/Delete/5
            [HttpPost, ActionName("Delete")]
            [ValidateAntiForgeryToken]
            public ActionResult DeleteConfirmed(int id)
            {
                Game game = unitOfWork.GameRepository.GetByID(id);
                unitOfWork.GameRepository.Delete(id);
                unitOfWork.Save();
                return RedirectToAction("Index");
            }

            protected override void Dispose(bool disposing)
            {
                if (disposing)
                {
                    unitOfWork.Dispose();
                }
                base.Dispose(disposing);
            }
        }
    }
<small>_Controllers/GameController.cs file edited_</small>

Here, we add a class variable for the "_UnitOfWork_". One improvement we could do here is to use the _constructor_ instead or to use _Dependency Injection_. The main change is that every reference to the database _Context_ is replaced by a reference to the appropriate _Repository_, using "_UnitOfWork_" properties to access the _Repository_.

If we run our application again, we see that it looks and works the same as before.

## Conclusion ##

Through this article, we defined the _Repository_ and the _Unit of Work_ patterns. We saw which concept they encapsulate and how we can use them in our application.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!

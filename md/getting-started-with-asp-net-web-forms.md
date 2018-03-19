# Getting Started with ASP.NET Web Forms #

In this article, we are going to take a look at _ASP.NET Web Forms_.

##  Introduction ##

Through this post, we are going to build a really simple application using the _ASP.NET Web Forms_ technology. It is going to be pretty straightforward and this will allow us to have a brief overview of this tool.

## What is ASP.NET Web Forms? ##

_ASP.NET_ offers us a few ways to create dynamic web applications and web sites. _ASP.NET Web Forms_ is one of them.

As we said, with Web Forms, we can create web applications or web sites. Web applications need to be compiled before deployment, while web sites are compiled and executed on the server, which generates the _HTML_ that displays the web pages.

### Vocabulary ###

Before diving into our project, let's take a moment to see a little bit of vocabulary.

### Pages ###

In an _ASP.NET Web Forms_, a page looks like an _HTML_ page. The extension of the file is "_.aspx_". When a browser requests an _ASP.NET_ page, the server processes any executable code in the page, before the result is sent back to the browser.

### Directives ###

A directive is a special instruction on how _ASP.NET_ should process the page.

### Controls ###

_Controls_ are tags that the server understands.

There are three kinds of server _Controls_:

* HTML Server Controls - Traditional _HTML_ tags
* Web Server Controls - New _ASP.NET_ tags
* Validation Server Controls - For input validation

### Forms ###

All _Controls_ must be embedded in a _Form_. The "_form_" tag must contain the "_runat="server"_" attribute that indicates that the form should be processed on the server.

### Master Page ###

A _Master Page_ allows us to create a consistent layout for the pages in our application.

### Events ###

In an _ASP.NET Web Forms_ application, working with _Server Controls_ on the page involves responding to _Events_ raised by _Controls_. To control _Events_, we need to handle them with our own code that we will bind to the specific _Events_.

### Post Back ###

A _Post Back_ occurs when we send data back to the server via a _POST request_. It is the name given to the process of submitting an _ASP.NET_ page to the server for processing. 

### View State ###

The _View State_ is the method to preserve the value of the _Page_ and _Controls_. It stores the value of the _Pages_ at the time of _Post Back_.

### Code-Behind ###

_Code-Behind_ refers to the code that relates to a specific page and stored in another file with the "_.cs_" extension.

### Data binding ###

Basically, data binding allows properties of two objects to be linked so that a change in one causes a change in the other.

Data binding is used to simplify how an application displays and interacts with its data. It establishes a connection between the user interface and the underlying application. It defines a relationship between two objects: a source object that will provide data and a target object that will use the data from the source object.

The benefit of data binding is that we no longer have to worry about synchronizing data between our views and data source. Most of the time, bindings are used to connect the visuals of an application with an underlying data model.

## Page life-cycle stages ##

In an _ASP.NET Web Forms_, when a Page is requested, it goes through a life-cycle in which various processing steps are performed. Here are the different steps that are part of this life-cycle.

* Page request: occurs before the page life cycle begins. It determines whether the page needs to be parsed and compiled.
* Start: it sets page properties such as _Request_ and Response. It also determines whether the _Request_ is a _Post Back_ or a new _Request_.
* Initialization: it sets the ID of each _Control_ while they are available.
* Load: if the current _Request_ is a _Post Back_, _Control_ properties are loaded with information recovered from _View State_ and _Control State_.
* Postback event handling: if the _Request_ is a _Post Back_, _Control Event_ handlers are called.
* Rendering: it saves _View State_ for the _Page_ and the _Controls_, then each element is rendered.
* Unload: it is raised after the page has been fully rendered, sent to the client, and is ready to be discarded.

## Creating our project ##

Let's head to _Visual Studio_, select "_New Project > Visual C# > Web > ASP.NET We Application (.NET Framework)_" and name our application "_MusicApplication_". Then, we have to select the "_Web Forms_" option. We can check the box for unit tests if we want to generate a second assembly for unit testing.

## Project structure ##

Now that our application is generated, let's take a look at the different folders we have.

### Application information ###

* Properties - application properties (application type, startup object, assembly information)
* References - used to pull additional libraries into the project

### Application folders ###

* App_Data - application data (for example, SQL database)
* App_Start - routes and bundles
* Content - static files like CSS files, icons or images

### Configuration files ###

* Global.asax - contains code for responding to application-level events raised by _ASP.NET_ or by HttpModules
* packages.config - is managed by the NuGet infrastructure and tracks installed packages with their respective versions
* Web.config - contains settings that apply to the web site

## First run ##

Now, let's hit "_F5_", or click the "play" button in _Visual Studio_ and wait a few seconds. This will start _IIS Express_ and run our web app. _Visual Studio_ then launches a browser and opens the application's home page. If everything is alright, our application will be available at "_http://localhost:RANDOM-PORT_".

## Creating a Data Model ##

First, we are going to define a _Data Model_ for our application. We are going to need the _Entity Framework_ and LocalDB.

The _Entity Framework_ is an _object-relational mapper_ (_ORM_) that allows us to work with a database using _.NET_ objects. It supports a development paradigm called _Code First_. It lets us create _Model_ objects by writing simple classes (also known as _POCO_ classes, from "_plain-old CLR objects_"). We can have the database created on the fly from our classes.

_LocalDB_ is a lightweight version of the _SQL Server Express Database Engine_. _LocalDB_ runs in a special execution mode of _SQL Server Express_ that allows us to work with databases as "_.mdf_" files. _LocalDB_ is installed by default with _Visual Studio_.

First, we need to install the _Entity Framework_. We can achieve this by calling the graphic _Package Manager_ or using the command line version like so:

    PM> Install-Package EntityFramework -Version 6.2.0 
<small>_Installing Entity Framework_</small>

Now, let's create a new folder named "_Models_". Then, in that same folder, let's create a class named "_Band_" (_"Add > New Item > Code > Class"_) and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;
    using System.Linq;
    using System.Web;

    namespace MusicApplication.Models
    {
        public class Band
        {
            [ScaffoldColumn(false)]
            public int Id { get; set; }

            [Required, StringLength(100), Display(Name = "Name")]
            public string BandName { get; set; }

            [Required, StringLength(10000), Display(Name = "Band Biography"), DataType(DataType.MultilineText)]
            public string Bio { get; set; }
        }
    }
<small>_Models/Band.cs file_</small>

As we can see, we use _Data Annotations_ that allow us to describe how to validate user input for a class member, to specify formatting for it, and to specify how it is modelled when the database is created.

## Creating the Context ##

Let's create a "_DAL_" folder. In that folder, let's create a class named "_BandContext_" and fill it like so:

    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Web;
    using MusicApplication.Models;

    namespace MusicApplication.DAL
    {
        public class BandContext : DbContext
        {

            public BandContext() : base("BandContext")
            {
            }

            public DbSet<Band> Bands { get; set; }
        }
    }
<small>_DAL/BandContext.cs file_</small>

First, we pass the name of the connection string to the _constructor_.

Then, and because it is just a really simple example, we create only one "_DbSet_" property. A "_DbSet_" corresponds to a database table and an _Entity_ corresponds to a row in the table.

## Setting up Entity Framework to use LocalDB ##

Let's edit our "_Web.config_" file and put the following lines in to that one:

    <connectionStrings>
        <add name="BandContext" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=MusicApplication;Integrated Security=SSPI;" providerName="System.Data.SqlClient"/>
    </connectionStrings>
<small>_Web.config file edited_</small>

## Migrations ##

In a real application, our _Data Model_ will change frequently. So, for each change, we need to sync that database. We could configure the _Entity Framework_ to automatically drop and re-create the database each time we change the _Data Model_. This is alright for development, but not for production because we would lose data. To solve this problem, we need to enable the _Code First Migrations_ feature.

We can enable _Migrations_ like so:

    PM> enable-migrations
<small>_Enabling Migrations_</small>

As we can see, a "_Migrations_" folder has been created. In this folder, we can find a file "_Configuration_" that contains a "_Seed()_" method. Let's place the following code into that file:

    namespace MusicApplication.Migrations
    {
        using System;
        using System.Data.Entity;
        using System.Data.Entity.Migrations;
        using System.Collections.Generic;
        using System.Linq;
        using MusicApplication.Models;

        internal sealed class Configuration : DbMigrationsConfiguration<MusicApplication.DAL.BandContext>
        {
            public Configuration()
            {
                AutomaticMigrationsEnabled = false;
            }

            protected override void Seed(MusicApplication.DAL.BandContext context)
            {
                var bands = new List<Band>
                {
                    new Band{BandName="Depeche Mode", Bio="A band from Basildon."},
                    new Band{BandName="The Beatles", Bio="A band from Liverpool."}
                };

                bands.ForEach(e => context.Bands.AddOrUpdate(p => p.BandName, e));
                context.SaveChanges();
            }
        }
    }
<small>_Migrations/Configuration.cs file_</small>

Now, we can create a _Migration_ like so:

    PM> add-migration InitialCreate
<small>_Creating a Migration_</small>

In the "_Migrations_" folder, we can now find the code that would create the database from scratch. The "_Up()_" method creates the database tables that correspond to the data model entity sets, and the "_Down()_" method deletes them.

To execute our _Migration_, we can use the following code:

    PM> update-database -TargetMigration InitialCreate
<small>_Executing our Migration_</small>

We can use the _SQL Server Explorer_ to see that our _Migration_ has been run.

## Displaying data ##

We can now create the different views we need to display our data. First, let's create a view named "_BandList_" ("_Add > New Item > Web > Web Forms > Web Form_") and fill it like so:

    <%@ Page Title="Bands" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" 
            CodeBehind="BandList.aspx.cs" Inherits="MusicApplication.BandList" %>
    <asp:Content ID="Content1" ContentPlaceHolderID="MainContent" runat="server">
        <section>
            <div>
                <hgroup>
                    <h2><%: Page.Title %></h2>
                </hgroup>

                <asp:ListView ID="bandList" runat="server" 
                    DataKeyNames="Id" GroupItemCount="4"
                    ItemType="MusicApplication.Models.Band" SelectMethod="GetBands">
                    <EmptyDataTemplate>
                        <table >
                            <tr>
                                <td>No data was returned.</td>
                            </tr>
                        </table>
                    </EmptyDataTemplate>
                    <EmptyItemTemplate>
                        <td/>
                    </EmptyItemTemplate>
                    <GroupTemplate>
                        <tr id="itemPlaceholderContainer" runat="server">
                            <td id="itemPlaceholder" runat="server"></td>
                        </tr>
                    </GroupTemplate>
                    <ItemTemplate>
                        <td runat="server">
                            <table>
                                <tr>
                                    <td>
                                        <a href="BandDetails.aspx?Id=<%#:Item.Id%>">
                                            <span>
                                                <%#:Item.BandName%>
                                            </span>
                                        </a>
                                    </td>
                                </tr>
                            </table>
                            </p>
                        </td>
                    </ItemTemplate>
                    <LayoutTemplate>
                        <table style="width:100%;">
                            <tbody>
                                <tr>
                                    <td>
                                        <table id="groupPlaceholderContainer" runat="server" style="width:100%">
                                            <tr id="groupPlaceholder"></tr>
                                        </table>
                                    </td>
                                </tr>
                                <tr>
                                    <td></td>
                                </tr>
                                <tr></tr>
                            </tbody>
                        </table>
                    </LayoutTemplate>
                </asp:ListView>
            </div>
        </section>
    </asp:Content>
<small>_BandList.aspx file_</small>

We can see that there are various curious symbols in our code. Let's take a look at their meaning:

* <% %> - inline code (especially logic flow)
* <%$ %> - evaluating expressions (like resource variables)
* <%@ %> - Page directives, registering assemblies, importing namespaces, etc.
* <%= %> - short-hand for Response.Write
* <%# %> - used for data binding expressions.
* <%: %> - short-hand for Response.Write(Server.HTMLEncode()) ASP.NET 4.0+
* <%#: %> - used for data binding expressions and is automatically HTMLEncoded
* <%-- --%> - is for server-side comments

We now have to add the _code-behind_ file. For that, we need to unfold the "_BandList.aspx_" file by clicking on the little arrow next to it. In the file "_BandList.aspx.cs_", let's put to following code:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.ModelBinding;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using MusicApplication.DAL;
    using MusicApplication.Models;

    namespace MusicApplication
    {
        public partial class BandList : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            public IQueryable<Band> GetBands()
            {
                var _db = new MusicApplication.DAL.BandContext();
                IQueryable<Band> query = _db.Bands;
                return query;
            }
        }
    }
<small>_BandList.aspx.cs file_</small>

As we can see, here we have a method named "_GetBands()_" that is used in the "_BandList.aspx_" file in the "_ListView_" _Control_.

Let's create another view named "_BandDetails.aspx_" and place the following code into it:

    <%@ Page Title="Band Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" 
            CodeBehind="BandDetails.aspx.cs" Inherits="MusicApplication.BandDetails" %>
    <asp:Content ID="Content1" ContentPlaceHolderID="MainContent" runat="server">
        <asp:FormView ID="bandDetail" runat="server" ItemType="MusicApplication.Models.Band" SelectMethod ="GetBand" RenderOuterTable="false">
            <ItemTemplate>
                <div>
                    <h1><%#:Item.BandName %></h1>
                </div>
                <br />
                <table>
                    <tr>
                        <td style="vertical-align: top; text-align:left;">
                            <b>Biography:</b><br /><%#:Item.Bio %>
                        </td>
                    </tr>
                </table>
            </ItemTemplate>
        </asp:FormView>
    </asp:Content>
<small>_BandDetails.aspx file_</small>

Now, let's add the _code-behind_:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.ModelBinding;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using MusicApplication.DAL;
    using MusicApplication.Models;

    namespace MusicApplication
    {
        public partial class BandDetails : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            public IQueryable<Band> GetBand([QueryString("Id")] int? Id)
            {
                var _db = new MusicApplication.DAL.BandContext();
                IQueryable<Band> query = _db.Bands;
                if (Id.HasValue && Id > 0)
                {
                    query = query.Where(p => p.Id == Id);
                }
                else
                {
                    query = null;
                }
                return query;
            }
        }
    }
<small>_BandDetails.aspx.cs_</small>

As we can see, here, we have a method called "_GetBand()_" that takes an _ID_ as an argument. This method is used in our file "_BandDetails.aspx_".

## Adding routes ##

It is time to work on our URLs. For now, we can access to our views using "_http://localhost:RANDOM-PORT/BandList_" and something like "_http://localhost:RANDOM-PORT/BandDetails?Id=1_".

Let's start by adding the following code in the "_RouteConfig_" file:

    public static void RegisterCustomRoutes(RouteCollection routes)
    {
        routes.MapPageRoute(
            "BandById",
            "Band/{Id}",
            "~/BandDetails.aspx"
        );
    }
<small>_App_Start/RouteConfig.cs file edited_</small>

We also have to edit our "_Global.asax.cs_" file:

    namespace MusicApplication
    {
        public class Global : HttpApplication
        {
            void Application_Start(object sender, EventArgs e)
            {
                RouteConfig.RegisterRoutes(RouteTable.Routes);
                RouteConfig.RegisterCustomRoutes(RouteTable.Routes);
                BundleConfig.RegisterBundles(BundleTable.Bundles);
            }
        }
    }

<small>_Global.asax.cs file edited_</small>

Now, head to our "_BandList.apsx_" file and change our links like so:

    <a href="<%#: GetRouteUrl("BandById", new {Id = Item.Id}) %>">
        <span>
            <%#:Item.BandName%>
        </span>
    </a>
<small>_BandList.apsx file edited_</small>

Finally, we have to edit our "_BandDetails_" like so:

    ...
    using System.Web.Routing;
    ...

    public IQueryable<Band> GetBand([RouteData] int? Id)
    {
        ...
    }
<small>_BandDetails.aspx.cs file edited_</small>

Now, if everything is alright, we should have something displayed if we go to "_http://localhost:RANDOM-PORT/Band/1_".

## Conclusion ##

Through this article, we got an overview of _ASP.NET Web Forms_. We saw the basic concepts it uses. We built a small application and saw the different options that we have to create a dynamic web application. 

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
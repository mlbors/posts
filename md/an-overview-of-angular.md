# An overview of Angular #

In this article, we are going to have a look at the _Angular_ framework. Let's get into it!

## Introduction ##

Nowadays, we have plenty of options to develop something that is ready for various platforms. However, _Angular_ has made its way and it is now one of the most important actors. Let's see what it is and how it works.

We could jump right into the code of a project, but we would probably miss a few things. So, here, we are going to look at the architecture of _Angular_ to understand the different concepts and elements this last one uses.

## What is Angular? ##

Now, when we talk about _Angular_, we talk about _Angular 2_ or _Angular 5_. _Angular_ is a complete rewrite of the _AngularJS_ framework. _Angular_ as a different approach from its predecessor.

_Angular_ allows us to build applications across all platforms. It is an open-source platform that uses _TypeScript_. In a few words, _TypeScript_ is a strict syntactical superset of _JavaScript_, and adds optional static typing to the language.

## Architecture overview ##

_Angular_ is written in _TypeScript_ and it implements core and optional functionality as a set of _TypeScript libraries_ that we can import.

An _Angular_ application has, for building blocks, a thing called _NgModules_. An _Angular_ app is defined by a set of _NgModules_. An app always has at least a _Root Module_ that enables bootstrapping. An _NgModule_ is made of _Components_. Every app has at least a _Root Component_.

_Components_, and things like _Services_, are just classes. They are, however, marked with decorators that tells _Angular_ how to use them.

_Angular_ provides a _Router Service_ that helps us to define navigation paths among the different _Views_.

## Modules ##

_Angular_ apps are modular and this modularity system is called _NgModules_.

An _NgModule_ defines a set of _Components_. An _NgModule_ associate related code to form functional units. Every _Angular_ app has a _Root Module_, conventionally named _AppModule_, which provides the bootstrap mechanism that launches the application.

Even if they are different and unrelated, _NgModules_, like _JavaScript_ modules, can import functionality from other _NgModules_, and allow their own functionality to be exported and used by other _NgModules_. What we call _Angular Libraries_ are _NgModules_.

We declare an _NgModule_ by decorating our class with the "_@NgModule_" decorator. This decorator is a metadata object whose properties describe the module. The most important properties, which are arrays, are the following:

* _declarations_ - _Components_, _Directives_, and _Pipes_ that belong to the _NgModule_
* _exports_ - the subset of declarations that should be visible and usable in the _Components_ of other _NgModules_
* _imports_ - other modules whose exported classes are needed by _Components_ declared in the _NgModule_
* _providers_ - list of the needed _Services_ that, because they are listed here, become are available app-wide
* _bootstrap_ - the main application _View_, called the _Root Component_, which hosts all other app views. (only the _Root Module_ should set this bootstrap property)

An _NgModule_ provides a compilation context for its various _Components_. So, the _Components_ that belong to an _NgModule_ share a compilation context. _NgModules_ define a cohesive block of functionality.

The _Root Module_ of our application is the one that we bootstrap to launch the application. The application launches by bootstrapping the root _AppModule_. We also call it the _entryComponent_. The bootstrapping process creates the _Components_ listed in the "_bootstrap_" array and inserts each one into the browser _DOM_. So, each bootstrapped _Component_ is the base of its own tree of _Components_.

As we saw, we can have a _Root Module_, but we can have what we call _Feature Modules_. A _Feature Module_ delivers a cohesive set of functionality focused on a specific application needs. We could do everything in the _Root Module_, but a _Feature Module_ will help us partition our app into focused areas. However, the structure of a _Feature Module_ is exactly the same as the one of a _Root Module_.

Down below, we can find an example of how could look an _NgModule_. Here, it is the _AppModule_:

    // Importing Angular Libraries
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    import { HttpModule } from '@angular/http';

    // Importing the AppComponent
    import { AppComponent } from './app.component';

    // Importing a custom feature module
    import { CustomFeatureModule } from './custom-feature-module/custom-feature-module.module';

    // Declaring the Module
    @NgModule({
        declarations: [
            AppComponent
        ],
        imports: [
            BrowserModule,
            FormsModule,
            HttpModule,
            CustomerDashboardModule
        ],
        providers: [],
        bootstrap: [AppComponent]
    })
    export class AppModule { }

## Components ##

A _Component_ controls a patch of screen called a _View_. The logic of a _Component_ is defined inside a class. _Angular_ creates, updates, and destroys _Components_ as the user moves through the application.

A _Component_ is identified by the "_@Component_" decorator that has a set of properties. The most import properties are the following ones:

* _selector_ - tells how the component is referenced in _HTML_; in simple words, it corresponds to the _HTML_ tag.
* _templateUrl_ - gives the path of the _HTML_ template.
* _providers_ - an array of _Dependency Injection Providers_ for _Services_ that the _Component_ requires.

Notice that instead of the "_templateUrl_" property, we could use the "_template_" property that lets us provide the _HTML_ template inline.

A _Component_ has a _View_ and this last one is defined through an _HTML_ template. This _HTML_ file also contains some syntactic elements that are included in _Angular_.

A _Component_ will typically look like so:

    @Component({
        selector:    'my-component',
        templateUrl: './my-component.component.html',
        providers:  [ MyService ]
    })
    export class MyComponent implements OnInit {
        // Some code
    }

Before going any further with _Components_, let's take a look at a few other elements to simplify some terms that we will use later.

## Services ##

A _Service_ is useful to define things that can't fit into a _Component_ and find their reason to exist in the _separation of concerns_. A _Service_ is a class with a well-defined purpose. For example, we should create a _Service_ when two or more _Components_ or other things need to access the same data or if we want to encapsulate interactions with a web server or if we want to define how to validate user inputs.

_Services_ are _Singletons_, so there is only one instance of each _Service_ we define. They are _stateless objects_ that can be invoked from any _Components_. Their purpose is to help us to divide our application into small, different logical units that can be reused.

A _Service_ is a simple class and could look like so:

    export class Logger {
        log(msg: any)   { console.log(msg); }
        error(msg: any) { console.error(msg); }
        warn(msg: any)  { console.warn(msg); }
    }

## Dependency Injection ##

_Dependency Injection_ is a large subject. _Dependency Injection_, also called _DI_, is a _Design Pattern_ in which one or more dependencies (_Services_) are injected into a dependent object (_Client_). This pattern allows us to implement a loosely coupled architecture by separating the creation of a client's dependencies from its own behavior. 

We can apply this pattern when we want to remove knowledge of concrete implementations from objects, but also when we want to get a better testable code in isolation using mock objects.

The _DI_ Pattern is commonly used to implement the _Inversion of Control Principle_, which in a few words, separates the _what-to-do_ part of the _when-to-do part_. In other words, it is about letting somebody else handles the flow of control. It is based on the _Hollywood Principle_: "_Don't call us, we'll call you_".

_Dependency Injection_ could be achieved by using the "_constructor_" of a class or "_setter_" methods. It can also be achieved with a _Container_ that handles the instantiation of other objects.

In _Angular_, _DI_ is widely used and we can take a moment to dig a little into it.

_Angular_ uses its own _Dependency Injection_ framework that basically uses three things:

* The _Injector_, that exposes _APIs_. It is responsible for creating _Service_ instances and injecting them into classes.
* The _Provider_, that tells the _Injector_ how to create an instance of a dependency.
* The _Dependency_, the type of which an object should be created.

_Angular_ has a _Hierarchical Dependency Injection_ system. There is a tree of _Injectors_ that parallels an application's _Component_ tree. An application may have multiple _Injectors_. That means we can configure _Providers_ at different levels:

* For the whole application when bootstrapping it. All sub _Injectors_ will see the _Provider_ and share the instance associated with. It will always be the same instance.
* For a specific _Component_ and its sub _Components_. Other _Components_ won't see the _Provider_.
* For _Services_. They use one of the _Injectors_ from the element that calls the _Service_ chain.

When using _DI_ with _Angular_, we will mainly see the "_@Injectable_" decorator. This decorator marks a class as available to _Injector_ for creation.

In an _Angular_ app, _Components_ consume _Services_. A _Component_ shouldn't create a _Service_. So, we inject the different required _Services_ into the different _Components_. When _Angular_ creates a new instance of a _Component_ class, it determines which _Services_ or other dependencies that _Component_ needs by looking at the types of its _constructor_ parameters. When _Angular_ discovers that a _Component_ needs a _Service_, it checks if the _Injector_ already has any existing instances of that same _Service_. If an instance of that requested _Service_ doesn't exist, the _Injector_ makes one using the registered _Provider_ and adds it to the _Injector_ before returning the _Service_ to _Angular_.

A _Provider_ is a recipe for creating a dependency. We must at least register one _Provider_ of any _Service_ we want to use. It can be done in Modules or in _Components_. Doing this in a _Module_ allows _Angular_ to inject the corresponding _Services_ in any class it creates and so the _Service_ instance lives for the life of the app. By using a _Component Provider_ we restrict the scope of the _Service_ and so it will only be injected into that _Component_ instance or one of its descendant _Component_ instances. It means that _Angular_ can't inject the same _Service_ instance anywhere else. The lifetime of this _Service_ will also be different: the _Service_ instance will be destroyed when the _Component_ instance is destroyed.

Here is how we can inject a _Service_ in a _Component_:

    import { Injectable } from '@angular/core';
    import { Logger } from './logger';

    @Injectable()
    export class Logger {
        log(msg: any)   { console.log(msg); }
        error(msg: any) { console.error(msg); }
        warn(msg: any)  { console.warn(msg); }
    }
<small>_logger.service.ts file_</small>

    import { Component } from '@angular/core';
    import { Logger } from './logger';

    @Component({
        selector:    'my-component',
        templateUrl: './my-component.component.html',
        providers:  [ Logger ]
    })
    export class HeroListComponent implements OnInit {
        constructor(private logger: Logger {}
    }
<small>_my-component.component.ts file_</small>

We could also do it with the _Root Module_ like so:

    @NgModule({
        providers: [
            Logger
        ]
    })
<small>_app.module.ts file_</small>

    import { Component } from '@angular/core';
    import { Logger } from './logger';

    @Component({
        selector:    'my-component',
        templateUrl: './my-component.component.html',
    })
    export class MyComponent implements OnInit {
        constructor(private logger: Logger {}
    }
<small>_my-component.component.ts file_</small>

We can also imagine the a _Service_ needs another _Service_:

    import { Injectable } from '@angular/core';
    import { Logger }     from './logger.service';

    @Injectable()
    export class MyService {
        constructor(private logger: Logger) {  }
    }

## Data Binding  ##

Basically, _data bindings_ allow properties of two objects to be linked so that a change in one causes a change in the other. It establishes a connection between the user interface and the underlying application. It defines a relationship between two objects: a source object that will provide data and a target object that will use the data from the source object. The benefit of _data binding_ is that we no longer have to worry about synchronizing data between our _Views_ and data source.

With _Angular_, the most common way to display a _Component_ property is to bind that property name through _interpolation_. _Interpolation_ is the estimation of a value or a set of values based on their context. It allows to evaluate a string containing one or more placeholders and to replace those placeholders with a computed and corresponding value. The context is typically the _Component_ instance. So, basically, in _Angular_, to achieve this, we have to put the property name in the _View_ enclosed in double curly braces. It will be something like so:

    <h1>{{title}}</h1>

Most of the time, _bindings_ are used to connect the visuals of an application with an underlying data model, usually in a realization of the _MVVM Pattern_ (_Model-View-ViewModel_) or the _MVC Pattern_ (_Mode-View-Controller_). In _Angular_, the _Component_ plays the part of the _Controller/ViewModel_, and the template represents the _View_.

_Angular_ provides many kinds of _data binding_. _Binding types_ can be grouped into three categories distinguished by the direction of the data flow: _source-to-view_, _view-to-source_ and _two-way sequence: view-to-source-to-view_. When we use _binding types_ other than _interpolation_, we have to specify a target name that is the name of a property. It looks like an attribute name, but it is not. With _data binding_, we are not working with _HTML_ attributes, but properties of _DOM_ (_Document Object Model_) elements. Just to refresh our minds, we may say that attributes are defined by _HTML_ and properties are defined by _DOM_ and the responsibility of _HTML_ attributes is just to initialize _DOM_ properties. Later _DOM_ properties can change, but _HTML_ attributes cannot. Some _DOM_ properties don't have corresponding attributes and some _HTML_ attributes don't have corresponding properties. The target of a _data binding_ is something in the _DOM_.

    import { Component } from '@angular/core';

    @Component({
        selector:    'my-component',
        templateUrl: './my-component.component.html',
    })
    export class MyComponent {
        imgSrc: String = 'path-to-image';
    }
<small>_my-component.component.ts file_</small>

    <img [src]="imgSrc">
<small>_my-component.component.html file_</small>

We often say that property binding is one-way _data binding_ because it flows a value in one direction, from a _Component_'s data property into a target element property. However, we are allowed to achieve something called two-way _data binding_ that, for example, lets us display a data property and update that property when the user makes changes. We can do this by using the syntax "_[(x)]_".

We are also able to achieve event binding:

    export class MyComponent {
        doSomething() {
            // some code
        }
    }
<small>_my-component.component.ts file_</small>

    <button (click)="doSomething()">Do something</button>
<small>_my-component.component.html file_</small>

## Input and Output ##

In a _Component_, we can use two decorators on properties: "_@Input_" and "_@Output_".

An _Input_ property is a settable property. An _Output_ property is an observable property. _Input_ properties usually receive data values. _Output_ properties expose _Event_ producers.

Declaring an _Input_ property would give something like so:

    export class MyComponent {
        @Input() name: String;
    }
<small>_my-component.component.ts file_</small>

    <my-component name="foo"></my-component>
<small>_my-component.component.html file_</small>

An Output property almost always returns an _Angular_ _EventEmitter_. An _EventEmitter_ allows us to emit a custom _Event_. It is helpful to pass a value to a parent _Component_. Let's say that we have something like this:

    export class MyComponent {
        @Output() deleteItemRequest = new EventEmitter<Item>();

        delete() {
            this.deleteItemRequest.emit(this.item)
        }
    }
<small>_my-component.component.ts file_</small>

    <button (click)="delete()">Delete</button>
<small>_my-component.component.html file_</small>

As we can see, here, we use event binding. So, when the button is clicked, we call the "_delete()_" method. In the _Component_, we also declare an _Output_ property that returns an _EventEmitter_ and we declare its underlying type as "_Item_". So, when the "_delete()_" method is called, we use this _EventEmitter_ to emit a new _Event_. In fact, it will emit an "_Item_" object.

So, we can now imagine that we have the following thing as a parent _Component_:

    export class ParentComponent {
        deleteItem(item: Item) {
            // Some code
        } 
    }
<small>_parent-component.component.ts file_</small>

    <parent-component (deleteItemRequest)="deleteItem($event)"></parent-component>
<small>_parent-component.component.ts file_</small>

When the child _Component_ emits its _Event_, the parent _Component_ will use the result of this same _Event_ with its own method.

## Component Lifecycle Hooks ##

_Angular_ manages the lifecycle of the different _Components_. Through different _Hooks_, it provides a way to perform actions when those different moments occur. We access to those moments by implementing one or more of the lifecycle _Hook_ interfaces in the _Angular_ core library. Each interface has a single _Hook_ method whose name is the interface name prefixed with "_ng_".

Down below, we have an example of a _Component_ using the "_OnInit_" interface:

    export class MyComponent implements OnInit {
        ngOnInit() {
            // Some code
        }
    }

## Communication between parent and child Components ##

There are a few ways to make a parent and child _Component_ interact. One way is to inject the child _Component_ into the parent as a _ViewChild_. This could be achieved like so:

    import { ViewChild }       from '@angular/core';
    import { Component }       from '@angular/core';
    import { ChildComponent }  from './child-component.component';

    export class ParentComponent {
        @ViewChild(ChildComponent)
        private childComponent: ChildComponent;

        method1() {
            this.childComponent.childMethod1();
        }

        method2() {
            this.childComponent.childMethod2();
        }
    }

Another way to make a parent and child _Component_ interact is to make them share a _Service_.

## Directives ##

In _Angular_, there are three kinds of _Directives_:

* _Components_ - _Directives_ with a template
* _Structural Directives_ - change the _DOM_ layout by adding and removing _DOM_ elements
* _Attribute Directives_ - change the appearance or behavior of an element, _Component_, or another _Directive_

We have already seen _Components_. They are the most common _Directives_.

_Structural Directives_ change the structure of the _View_. They are things like "_NgFor_" or "_NgIf_". Here is an example of different _Structural Directives_:

    <div *ngIf="character" class="name">{{character.name}}</div>

    <ul>
        <li *ngFor="let character of characters">{{character.name}}</li>
    </ul>

    <div [ngSwitch]="character?.size">
        <app-big-character    *ngSwitchCase="'big'"       [character]="character"></app-big-character>
        <app-medium-character *ngSwitchCase="'medium'"    [character]="character"></app-medium-character>
        <app-small-character  *ngSwitchCase="'small'"     [character]="character"></app-small-character>
        <app-character        *ngSwitchDefault="'small'"  [character]="character"></app-character>
    </div>

_Attribute Directives_ are used as attributes of elements. They are things like "_NgClass_" or "_NgStyle_". Here is an example of different _Attribute Directives_:

    <div [ngStyle]="currentStyles">
        Some content.
    </div>

    <div [class.error]="hasError">Some error</div>

Let's make a little side note for the "_NgModel_" _Directive_ that is part of the "_FormsModule_". This _Directive_ helps us when we want to display a data property and update that property when the user makes changes through a form. Using this _two-way data binding_ makes this easier. It will map the various fields of our form to our _Data Model_. It will ensure that the data in the _View_ and the data in our _Data Model_ are synced.

We can use this _Directive_ like so:

    export class MyComponent {
        name: string;
    }
<small>_my-component.component.ts file_</small>

    <input type="text" [(ngModel)]="name" />
<small>_my-component.component.html file_</small>

We are also able to build _Attribute Directives_. We just have to create a class annotated with the "_@Directive_" decorator.

## Pipes ##

_Pipes_ are a way to operate some transformations over data before displaying them. _Angular_ comes with several built-in _Pipes_. For example, we can have something like so:

    <p>The character's birthday is {{ birthday | date:"MM/dd/yy" }}</p>

We are also able to create our own _Pipes_ by using the "_@Pipe_" decorator and implementing the "_PipeTransform_" interface. This could be done like so:

    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({name: 'exponentialStrength'})
    export class ExponentialStrengthPipe implements PipeTransform {
        transform(value: number, exponent: string): number {
            let exp = parseFloat(exponent);
            return Math.pow(value, isNaN(exp) ? 1 : exp);
        }
    }

## Observables ##

_Observables_ provide support for passing messages between _Publishers_ and _Subscribers_ in our application. An _Observable_ can deliver multiple values of any type.

A _Publisher_ must create an _Observable_ instance. This object defines a function that is executed when a consumer calls the "_subscribe()_" method. This method, called the _subscriber function_, states how to get data to be published. To execute our _Observable_, we have to call its "_subscribe()_" method and pass it an _Observer_. This object implements the "_Observer_" interface and is responsible to handle the various notifications from the _Observable_.

To use _Observables_, we need to import the _RxJS Library_. _RxJS_ is a library for reactive programming, which is a declarative programming paradigm where we program with asynchronous data streams. Data streams can be anything and we are so able to listen to them and react accordingly. A stream is a sequence of continuous _Events_ ordered in time and it can emit three different things: a value of some type, an error or a "_completed_" value. Asynchronously, we capture these different emitted events by defining functions: one that will execute when a value is emitted, another that will execute when an error is emitted and another one that will execute when "_completed_" is emitted. The action of listening to the stream is named "_subscribing_". The various functions we define are the "_Observers_" while the stream is the "subject" or the "_Observale_". This is the _Behavioral Design Pattern_ called the _Observer Pattern_. We also have to deal with the "_Operators_" which are the various pure functions, functions that always evaluate the same result value when we give them the same argument value, that will let us work on the emitted values.

This kind of programming is really helpful when we have to deal with various _UI Events_ related to data _Events_. It helps us to achieve real-time apps.

Let's imagine that we have a _Service_ that is responsible to fetch users:

    import { Observable } from 'rxjs/Rx'
    import { Injectable } from '@angular/core'
    import { Http, Response } from '@angular/http'

    @Injectable()
    export class UsersService  {

        constructor(public http: Http) {}

        public fetchUsers() {
            return this.http.get('/api/users').map((res: Response) => res.json())
        }
    }

Our method "_fetchUsers()_" returns an _Observable_, our subject. So, we can subscribe to our subject like so:

    import { Component } from '@angular/core'
    import { Observable } from 'rxjs/Rx'

    import { UsersService } from './users.service'
    import { User } from './user'

    @Component({
        selector: "my-component",
        templateUrl:  "./my-component.component.html",
        providers:  [ UsersService ]
    })
    export class MyComponent {
        public users: Observable<User[]>

        constructor(public usersServce: UsersService) {}

        public ngOnInit() {
            this.users = this.UsersService.fetchUsers()
        }
    }

In our template file, we have to do the following things:

    <ul class="user-list" *ngIf="(users | async).length">
        <li class="user" *ngFor="let user of users | async">
            {{ user.name }}
        </li>
    </ul>

We may also want to create an _Observable_ from a _Promise_. We can do it like so:

    const data = fromPromise(fetch('/api/endpoint'));

This create an _Observable_. To subscribe, we have to do the following thing:

    data.subscribe({
        next(response) { console.log(response); },
        error(err) { console.error('Error: ' + err); },
        complete() { console.log('Completed'); }
    });

Here, we achieve the process of subscription and as we can see, we define the three functions that we talked about a little earlier.

## Forms ##

We can use _Angular_ event bindings to respond to _Events_ that are triggered by user input. For example, we can imagine the following situation:

    export class MyComponent {
        values = '';

        onKey(event: any) {
            this.values += event.target.value;
        }
    }
<small>_my-component.component.ts file_</small>

    <input (keyup)="onKey($event)">
    <p>{{values}}</p>
<small>_my-component.component.html file_</small>

_Angular_ has also a whole "_Form_" library that helps us with many things. We can, for example, use it to add some validation rules to our forms.

    <input id="name" name="name" class="form-control" required minlength="4" [(ngModel)]="user.name" #name="ngModel" >

    <div *ngIf="name.invalid && (name.dirty || name.touched)" class="alert alert-danger">

        <div *ngIf="name.errors.required">
            Name is required.
        </div>
        <div *ngIf="name.errors.minlength">
            Name must be at least 4 characters long.
        </div>
    </div>

Here, we start by defining a input with a few rules. As we can see, we export the "_ngModel_" _Directive_ to achieve _two-way data binding_. We also export the form control's state to a local variable "_\#name_". Then, we check if the control has been touched and we display different errors if there are some.

With _Angular_, we also have the ability to dynamically generate forms. To achieve this, we have to create objects derived from the base class "_QuestionBase_" and that represents the various controls of our forms. We can then treat them through a _Service_ that will build the form and return it as a "_FormGroup_" object.

## Routing & Navigation ##

In _Angular_, the _Router_ allows navigation from one _View_ to the next. The _Router_ interprets a browser _URL_ to navigate to a client generated _View_ and, if needed, pass optional parameters. The _Router_ can be bound to links or it can be used in response to some actions.

To use the _Router_ correctly, we need to add a "_base_" element to our "_index.html_" file. We also need to import the _Router Module_. In our "_app.module.ts_" file, we can do the following thing:

    import { RouterModule, Routes } from '@angular/router';

    const appRoutes: Routes = [
        { path: 'characters',         component: CharactersComponent },
        { path: 'character/:id',      component: CharacterDetailComponent },
        { path: '', redirectTo: '/characters', pathMatch: 'full' },
        { path: '**',                 component: PageNotFoundComponent }
    ];

    @NgModule({
        imports: [
            RouterModule.forRoot(appRoutes)
        ]
    })
    export class AppModule { }

As we can see, we define our navigation _Routes_ in the array "_appRoutes_" and we pass this array to the "_RouterModule_". We can now use the "_RouterOutlet_" _Directive_, that marks where the _Router_ displays a _View_, to create some kind of navigation menu:

    <nav>
        <a routerLink="/characters" routerLinkActive="active">Characters</a>
    </nav>
    <router-outlet></router-outlet>

After the end of each successful navigation lifecycle, the _Router_ builds a tree of "_ActivatedRoute_" objects that make up the current state of the _Router_. We are able to access the current "_RouterState_" from anywhere in the application using the _Router Service_ and the "_routerState_" property.

## Conclusion ##

Through this article, we got a brief overview of the _Angular_ technology. It was more a theoretical post than a practical example. Of course, we didn't cover entirely each subject and there are plenty of other subjects that we could have explored like _Unit Testing_ or _E2E Testing_. Now, however, we have enough knowledge of _Angular_ to start a project and to dig deeper into this framework.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
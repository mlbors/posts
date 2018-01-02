# First steps with Xamarin #

Through this article, we are going to take a look at _Xamarin_ to try to understand how it works. To achieve this, we are going to make a very simple application. Let's get into it!

## Introduction ##

Nowadays, we have several tools to build simple or complex applications. Nevertheless, most of the time, there is one problem that we have to solve: our application has to run on different platforms and devices. One solution is to program the application as many times that we have platforms.

Another way is to use a solution that lets us build-cross platform applications. _Xamarin_ lets us go this way.

## What is Xamarin? ##

_Xamarin_ is a cross-platform development tool. We can use _C#_ to create _iOS_ and _Android_ apps. _Xamarin_ applications use native UIs on every platform. It is a cross-platform implementation of the _Microsoft .NET Framework_. It is part of this last one and available through _Visual Studio_.

Here we are going to use _Xamarin.Forms_. _Xamarin.Forms_ is a framework that allows us to create cross-platform user interfaces that can be shared across _Android_, _iOS_, _Windows_ and _Windows Phone_. The user interfaces are rendered using the native controls of the target platform, allowing _Xamarin.Forms_ applications to retain the appropriate look and feel for each platform.

Applications written using _Xamarin.Forms_ are able to use any of the _API's_ of the underlying platform. It is also possible to create applications that will have parts of their user interface created with _Xamarin.Forms_ while other parts use the native UI toolkit.

## Getting our tools ##

The first thing we need to do is to download [Visual Studio](https://www.visualstudio.com/) and to install it.

## Starting our project ##

First, we need to create a new project. We are going to choose "_Multiplatform_", "_App_". Then, we are going to select "_Xamarin.Forms_" and then "_Blank Forms App_". Let's call this application "_SampleApp_".

In the next step, we are going to select "_iOS_" and "_Android_" as targets and we are going to use "_Portable Class Library_".

## Looking at the App file ##

_Xamarin.Forms_ applications have a single class named "_App_" that is responsible for instantiating the first _Page_ that will be displayed.

    using Xamarin.Forms;

    namespace SampleApp
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new SampleAppPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
<small>_App.xaml.cs_ file</small>

## Creating our main Content Page ##

A _Page_ represents an _Activity_ in _Android_, a _View Controller_ in _iOS_ or a _Page in the Windows Universal_ Platform (_UWP_). _Xamarin.Forms Pages_ represent cross-platform mobile application screens.

Let's define our _XAML_ by opening the file named _SampleAppPage.xaml_ and editing it:

    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage 
        xmlns="http://xamarin.com/schemas/2014/forms" 
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
        xmlns:local="clr-namespace:SampleApp" 
        x:Class="SampleApp.SampleAppPage">

        <ContentPage.Content>
            <ScrollView Padding="15">
                <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
                    <Label Text="This is a cross platform app!" VerticalOptions="Center" HorizontalOptions="Center"/>
                </StackLayout>
            </ScrollView>
        </ContentPage.Content>

    </ContentPage>
<small>_SampleAppPage.xaml_ file</small>

There are two techniques to create user interfaces in _Xamarin.Forms_. The first one is to create UIs with _C#_. The second one is to use _Extensible Application Markup Language_ (_XAML_).

_XAML_ is a declarative markup language that is used to describe user interfaces. _XAML_ will play well with _MVVM_ (_Model-View-ViewModel_). _XAML_ defines the _View_ that is linked to _View Model_ code through _XAML_-based data bindings.

In our previous _XAML_ code we are using several elements, like a _StackLayout_ to arrange or unique _Label_ on the screen.

Now, let's assure that our _C#_ code looks like this:

    using System;
    using System.Collections.Generic;

    using Xamarin.Forms;

    namespace SampleApp
    {
        public partial class SampleAppPage : ContentPage
        {
            public SampleAppPage()
            {
                InitializeComponent();
            }
        }
    }
<small>_SampleAppPage.xaml.cs_ file</small>

## Our first build ##

Now, if we click and the little "play button" in _Visual Studio_ and wait for a couple of seconds, our application will be displayed in an _iOS_ or in an _Android_ simulator, depending on what we chose in the options list next to the run button.

## A closer look at XAML ##

As we said, _XAML_ is a declarative markup language created by _Microsoft_ extensively used in _.NET Framework _and it is part of _Xamarin.Forms_. _XAML_ is basically _XML_, but has some unique syntax features like property elements, attached properties and markup extensions.

In a _Xamarin.Forms_ application, a _XAML_ file is always associated with a C# code file that provides code support for the markup.

In the first lines of our _XAML_ file, we set some namespaces. Then, the "_x:Class_" attribute specifies a fully qualified _.NET_ class name meaning this _XAML_ file defines a new class named _SampleAppPage_ in the _SampleApp_ namespace that derives from _ContentPage_.

Within the _XAML_ file, classes and properties are referenced with _XML_ elements and attributes.

We also have a _ContentPage.Content_ tag. It is called property element tag.

_XAML_ also has markup extensions such as StaticResource, that returns an object from a resource dictionary, or "_x:Static_" that access a public static field, a public static property, a public constant field or an enumeration member.

## Data binding ##

Basically, data bindings allow properties of two objects to be linked so that a change in one causes a change in the other.

Data binding is used to simplify how a _Xamarin.Forms_ application displays and interacts with its data. It establishes a connection between the user interface and the underlying application. It defines a relationship between two objects: a source object that will provide data and a target object that will use the data from the source object.

The benefit of data binding is that we no longer have to worry about synchronizing data between our views and data source.

The "_Binding Context_" property of the target object must be set to the source object and the "_SetBinding_" must be called on the target object method to bind a property of that object to a property of the source object.

Most of the time, bindings are used connect the visuals of an application with an underlying data model, usually in a realization of the _MVVM_ (_Model-View-ViewModel_).

We can define data bindings in _XAML_ also. Sometimes it is set from the code-behind file, sometimes using a "_StaticResource_" or "_x:Static_" markup extension, and sometimes as the content of "_BindingContext_" property-element tags. For example, we can do something like this:

    <Slider x:Name="slider"
        Maximum="360"
        VerticalOptions="CenterAndExpand" />

    <Label BindingContext="{x:Reference slider}"
        Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}" />
<small>Data bings example</small>

A single view can have data bindings on several of its properties. However, each view can have only one "_Binding Context_", so multiple data bindings on that view must all reference properties of the same object.

The solution to this problem involves the "_Mode_" property that can have the following values:

* Default
* OneWay — values are transferred from the source to the target
* OneWayToSource — values are transferred from the target to the source
* TwoWay — values are transferred both ways between source and target

It is also easy to achieve data bindings with a _ListView_:

    <ListView ItemSource="{Binding ListOfItems}" ...>
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.View>
                        <Label Text="{Binding Name}" />
                    </ViewCell.View>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
<small>ListView example</small>

## A word about MVVM ##

We previously talked about the _MVVM Pattern_. Let's refresh our mind!

The _MVVM Pattern_ was developed by _Microsoft_. It is similar to _MVC Pattern_ (_Model-View-Controller_) and it is a way to separate our user interface from our logic. In a few words, this pattern is composed of the following elements:

### View ###

The _View_ holds the user interface code. It defines the structure, layout, and appearance of what the user will see on the screen.

### Model ###

The _Model_ contains the logic to perform business functions. It includes a data model along with business and validation logic.

### View Model_ ###

The _View Model_ is used to communicate between the _View_ and the _Model_. It exposes public properties and commands. It is responsible for handling the _View_ logic. It will interact with the _Model_ by invoking methods of this last one. It provides data from the _Model_ that the _View_ can easily use. It will retrieve data from the _Model_ and then makes the data available to the _View_.

### Binder ###

Binding mechanism is not a part of the pattern itself. It is an underlying technology but it is required for applying the _MVVM_ pattern. Basically, the _Binder_ interprets bindings defined in the UI, observes the _View Model_ for changes and updates the _View_ and it observes the _View_ for changes updates the _View Model_. Here, the _Binder_ is _XAML_.

### The pattern ###

The _MVVM_ pattern was invented with _XAML_ in mind. The View and the _View Model_ are often connected through data bindings defined in the _XAML_ file. The "_Binding Context_" for the _View_ is usually an instance of the "_ViewModel_".

_View Models_ generally implement the "_INotifyPropertyChanged_" interface. It means that the class fires an event called "_PropertyChanged_" whenever one of its properties changes. The data binding mechanism in _Xamarin.Forms_ attaches a handler to this "_PropertyChanged_" event so it can be notified when a property changes and keep the target updated with the new value.

Sometimes the _View_ needs to contain buttons that trigger various actions in the _View Model_ but that last one must not contain "clicked handlers" for the buttons because that would tie the _View Model_ to a particular user-interface paradigm.

To allow _View Models_ to be more independent of particular user interface objects but still allow methods to be called within the _View Model_, a "Command Interface" exists. This "_Command Interface_" is supported by the following elements:

* Button
* MenuItem
* ToolbarItem
* SearchBar
* TextCell
* ImageCell
* ListView
* TapGestureRecognizer

The "_ICommand_" interface defines two methods and one event:

* void Execute(object arg)
* bool CanExecute(object arg)
* event EventHandler CanExecuteChanged

## Adding more pages ##

Knowing what we know, let's continue to build our application. First, let's add more pages.

To achieve this, we now have to edit our "_App file_" like so:

    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace SampleApp
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new SampleAppPage());
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }

<small>_App.xaml.cs_ file</small>

First, let's take a closer look at that little guy at the top of the file:

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]

Here, we are making a little bit of optimization, choosing _XAML_ to be compiled, rather than to be interpreted.

Now, let's add another "_Forms ContentPage Xaml_" to our solution and call it "_SecondPage_". We are not going to touch the code-behind but only edit the _XAML_ file like so:

    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage 
        xmlns="http://xamarin.com/schemas/2014/forms" 
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
        x:Class="SampleApp.SecondPage">
        
        <ContentPage.Content>
            <ScrollView Padding="15">
                <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
                    <Label Text="I am another page" VerticalOptions="Center" HorizontalOptions="Center"/>
                </StackLayout>
            </ScrollView>
        </ContentPage.Content>
        
    </ContentPage>
<small>_SecondPage.xaml_ file</small>

We now have to edit our _SampleAppPage.xaml_ like so:

    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage 
        xmlns="http://xamarin.com/schemas/2014/forms" 
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
        xmlns:local="clr-namespace:SampleApp" 
        x:Class="SampleApp.SampleAppPage">

        <ContentPage.Content>
            <ScrollView Padding="15">
                <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
                    <Label Text="This is a cross platform app!" VerticalOptions="Center" HorizontalOptions="Center"/>
                    <Button Text="Second page" HorizontalOptions="Center" VerticalOptions="Center" Clicked="OnCallSecondPage" />
                </StackLayout>
            </ScrollView>
        </ContentPage.Content>

    </ContentPage>
_SampleAppPage.xaml_ file edited

And in our code-behind, we are going to add the following things:

    ...
    async void OnCallSecondPage(object sender, EventArgs e)
    {
        await Navigation.PushAsync(new SecondPage());  
    }
    ...
<small>_SampleAppPage.xaml.cs_ file edited</small>

We can now run our application and check if we have a second page.

## Interactions and data ##

We are now going to improve a little more our application. We are going to add a simple functionality that loads an image from a _REST API_ and display it on the screen.

First of all, we need to install two packages: "_System.Net.Http_" and "_Newtonsoft.Json_". When it is done, we create a simple class called "_WebImage_" with the following code:

    using System;
    namespace SampleApp
    {
        public class WebImage
        {
            public string File { get; set; }
        }
    }
<small>_WebImage.cs_ file</small>

Then, we can edit our _SampleAppPage.xaml_ like so:

    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage 
        xmlns="http://xamarin.com/schemas/2014/forms" 
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
        xmlns:local="clr-namespace:SampleApp" 
        x:Class="SampleApp.SampleAppPage"
        Title="Sample App">

        <ContentPage.BindingContext>
            <local:ViewModel/>
        </ContentPage.BindingContext>

        <ContentPage.Content>
            <ScrollView Padding="15">
                <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
                    <Label Text="This is a cross platform app!" VerticalOptions="Center" HorizontalOptions="Center"/>
                    <Image x:Name="mainImage" Source="{Binding ImgUrl}" />
                    <Button Text="Refresh" HorizontalOptions="Center" VerticalOptions="Center" Command="{Binding RefreshCommand}" />
                    <ActivityIndicator IsRunning="{Binding IsBusy}" />
                    <Button Text="Second page" HorizontalOptions="Center" VerticalOptions="Center" Clicked="OnCallSecondPage" />
                </StackLayout>
            </ScrollView>
        </ContentPage.Content>

    </ContentPage>
<small>_SampleAppPage.xaml_ file edited</small>

Our code-behind will be like so:

    using System;

    using Xamarin.Forms;

    namespace SampleApp
    {
        public partial class SampleAppPage : ContentPage
        {
            public SampleAppPage()
            {
                InitializeComponent();
            }

            async void OnCallSecondPage(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new SecondPage());  
            }
        }
    }
<small>_SampleAppPage.xaml.cs_ file edited</small>

Finally, we create a file called _ModelView_ and fill it with the following code:

    using System;
    using System.ComponentModel;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Windows.Input;

    using Xamarin.Forms;

    using Newtonsoft.Json;

    namespace SampleApp
    {
        public class ViewModel:INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            string imgUrl;
            bool busy = false;

            // Constructor
            public ViewModel()
            {
                // Defining Refresh Command
                RefreshCommand = new Command(async () => await DownloadAsync());
                // Loading data when page is loaded
                Task.Run(async () => await DownloadAsync());  
            }

            // Public properties
            public bool IsBusy
            {
                get { return busy; }
                set
                {
                    if (busy == value)
                    {
                        return; 
                    }
                    
                    busy = value;
                    OnPropertyChanged("IsBusy");
                }
            }

            public string ImgUrl
            {
                get { return imgUrl; }
                set
                {
                    if (imgUrl == value)
                    {
                        return;
                    }

                    imgUrl = value;
                    OnPropertyChanged("ImgUrl");
                }
            }

            // ICommand implementations
            public ICommand RefreshCommand { protected set; get; }

            protected virtual void OnPropertyChanged(string propertyName)
            {
                var changed = PropertyChanged;
                if (changed != null)
                {
                    PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
                }
            }

            async Task DownloadAsync()
            {
                IsBusy = true;
                var data = await Task.Run(() => Download());
                ImgUrl = data.File;
                IsBusy = false;
            }

            async Task<WebImage> Download()
            {
                HttpClient client = new HttpClient();
                var response = await client.GetStringAsync("http://random.cat/meow");
                var data = JsonConvert.DeserializeObject<WebImage>(response);
                return data;
            }
        }
    }
<small>_ModelView.cs_ file</small>

Now, if we run our application, we can see an image appear on the screen when the first page is loaded. With the "_Refresh_" button, we can get another image.

## Xamarin VS React Native VS Ionic ##

Comparing those technologies would take a whole article. But let's have a brief overview:

_React Native_ and _Ionic_ are two other famous solutions that help us build cross platform applications. The first difference is they both use _JavaScript_ while, obviously, _Xamarin_ uses _C#_. 

_React Native_ supports _Android_ and _iOS_, whereas _Xamarin_ also does _Windows Mobile_, _Android Wear_, _iOS Watch_, _Android TV_, _macOS_ and _Windows Universal App_ as well.

Some common controls in _React Native_ may need to be coded separately for each platform.

_React Native_ is a little low in performance when it comes to the development on _iOS_ platform.

_React Native_ works more like _Xamarin_ while _Ionic_ doesn’t use native widgets at all.

A complete comparison can be found [here](https://cruxlab.com/blog/reactnative-vs-xamarin/).

## Conclusion ##

Through this article, we got a little more familiar with _Xamarin_ by building a really simple application. Of course, we haven't covered everything and there are many things to explore. But we are now able to go deeper and to use this tool. By the way, [the following repository](https://github.com/xamarin/xamarin-forms-samples) is full of great examples!

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
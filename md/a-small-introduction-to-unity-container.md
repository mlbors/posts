# A small introduction to Unity Container #

Through this post, we are going to take a look at how we can achieve _Dependency Injection_ with _Unity_ (the _IoC Container_, not the _Game Engine_). Let's get into it!

## Introduction ##

_Dependency Injection_ is one way to implement _Inversion of Control_. _Dependency Injection_, also called _DI_, is a design pattern in which one or more dependencies (services) are injected into a dependent object (client). This pattern allows us to implement a loosely coupled architecture by separating the creation of a client's dependencies from its own behavior. We can use this pattern when we want to remove knowledge of concrete implementations from objects, but also when we want to get a better testable code in isolation using mock objects.

Here, we are going to see how we can achieve _Dependency Injection_ using _Unity_, the IoC Container, not the _Game Engine_. By way, one way to apply _Dependency Injection_ through our code when we work with _Unity3D_ (here, the _Game Engine_), is to use _Zenject_.

## Installing Unity ##

First, we need to install _Unity_. This can be done through the _NuGet Packets Manager_.

    PM> Install-Package Unity

## Creating a few interfaces ##

First, let's define a few interfaces:

    public interface IGame
    {
        void Play();
    }

    public interface IInitializer
    {   
    }

We can now create some concrete implementations for these two interfaces:

    public class PlatformGame : IGame
    {
        public void Play()
        {
            Console.WriteLine("Playing a platform game");
        }
    }

    public class RPGGame : IGame
    {
        public void Play()
        {
            Console.WriteLine("Playing an RPG game");
        }
    }

    public class Initializer : IInitializer
    {   
        public Initializer(IGame game)
        {
            game.Play();
        }
    }

## Using Unity ##

Now, let's wrap _Unity_ with another class. The classes that will use DI will then do so through what we can call a _Facade_. Our wrapper could look like so:

    public class DIWrapper
    {
        /*********************/
        /***** ATTRIBUTS *****/
        /*********************/

        /// <param name="_container">_Unity_ Container</param>

        protected static IUnityContainer _container;

        /**************************************************/
        /**************************************************/

        /***********************************/
        /***** CONTAINER GETTER/SETTER *****/
        /***********************************/

        public static IUnityContainer Container
        {
            get { return _container; }
            set { _container = value; }
        }

        /**************************************************/
        /**************************************************/

        /*********************/
        /***** CONSTRUCT *****/
        /*********************/

        static DIWrapper()
        {
            if (_container == null)
            {
                Container = new UnityContainer();
            }
        }

        /**************************************************/
        /**************************************************/

        /*******************/
        /***** RESOLVE *****/
        /*******************/

        /// <typeparam name="T">Type of object to return</typeparam>
        /// <summary>
        /// Resolves the type parameter T to an instance of the appropriate type.
        /// </summary>

        public static T Resolve<T>()
        {
            T result = default(T);

            if (Container.IsRegistered(typeof(T)))
            {
                result = Container.Resolve<T>();
            }

            return result;
        }

        /**************************************************/
        /**************************************************/

        /*******************/
        /***** RESOLVE *****/
        /*******************/

        /// <typeparam name="T">Type of object to return</typeparam>
        /// <param name="name">Object to resolve</param>
        /// <summary>
        /// Resolves the type parameter T to an instance of the appropriate type.
        /// </summary>

        public static T Resolve<T>(string name)
        {
            T result = default(T);

            if (Container.IsRegistered<T>(name))
            {
                return Container.Resolve<T>(name);
            }

            return result;
        }

        /**************************************************/
        /**************************************************/

        /********************/
        /***** REGISTER *****/
        /********************/

        /// <typeparam name="T">Type of object to return</typeparam>
        /// <param name="name">Object to resolve</param>
        /// <summary>
        /// Register the type parameter T.
        /// </summary>

        public static void Register<T>()
        {
            if (!Container.IsRegistered<T>())
            {
                Container.RegisterType<T>();
            }
        }

        /**************************************************/
        /**************************************************/

        /********************/
        /***** REGISTER *****/
        /********************/

        /// <typeparam name="TFrom">Type of object to return</typeparam>
        /// <summary>
        /// Register the type parameter T.
        /// </summary>

        public static void Register<TFrom, TTo>() where TTo : TFrom
        {
            if (!Container.IsRegistered<TFrom>())
            {
                Container.RegisterType<TFrom, TTo>();
            }
        }

        /**************************************************/
        /**************************************************/

        /********************/
        /***** REGISTER *****/
        /********************/

        /// <typeparam name="TFrom">Type of object to return</typeparam>
        /// <param name="name">Object to resolve</param>
        /// <summary>
        /// Register the type parameter T.
        /// </summary>

        public static void Register<TFrom, TTo>(string name) where TTo : TFrom
        {
            if (!Container.IsRegistered<TFrom>(name))
            {
                Container.RegisterType<TFrom, TTo>(name);
            }
        }
    }

Now, for example, in our "_Program.cs_", inside our "_Main_" method, we can imagine to have the follwing lines:

    static void Main(string[] args)
    {
        DIWrapper.Register<IGame, PlatformGame>();
        DIWrapper.Register<IInitializer, Initializer>();
        IInitializer initializer = DIWrapper.Resolve<IInitializer>();
    }

The first line means that we map the interface "_IGame_" to the concrete class "_PlatformGame_". So, each time we want to resolve an object of type "_IGame_", we will get an object from the "_PlatformGame_" class. We then do the samine with the "_IInitializer_" interface and then ask _Unity_ to resolve it. During the resolution, _Unity_ will know that IGame is mapped to "_PlatformGame_" and will inject it into the "_Initializer_" object, through the consructor we previously defined.

## A little futher with Factories ##

Now, let's imagine the following things:

    public interface IDIFactory<T>
    {
        T Create(params object[] constructorArguments);
    }

    public interface IGameFactory<T> : IDIFactory<T>
    {
        GameType Type
        {
            get;
            set;
        }
    }

    public enum GameType
    {
        Platform,
        RPG
    }

Now, let's imagine the following implementation:

    public class GameFactory<IGame> : IGameFactory<IGame>
    {
        /*********************/
        /***** ATTRIBUTS *****/
        /*********************/

        /// <param name="_type">Type of object to create</param>

        protected GameType _type;

        /**************************************************/
        /**************************************************/

        /******************************/
        /***** TYPE GETTER/SETTER *****/
        /******************************/

        public GameType Type
        {
            get { return _type; }
            set { _type = value; }
        }

        /**************************************************/
        /**************************************************/

        /*********************/
        /***** CONSTRUCT *****/
        /*********************/

        public GameFactory()
        {
            Type = GameType.Platform;
        }

        /**************************************************/
        /**************************************************/

        /******************/
        /***** CREATE *****/
        /******************/

        public IGame Create(params object[] constructorArguments)
        {
            IGame game;

            switch(_type)
            {
                case GameType.Platform:
                    DIWrapper.Register<CURRENT.NAMESPACE.IGame, PlatformGame>("Platform");
                    game = DIWrapper.Resolve<IGame>("Platform");
                    break;

                case GameType.RPG:
                    DIWrapper.Register<CURRENT.NAMESPACE.IGame, RPGGame>("RPG");
                    game = DIWrapper.Resolve<IGame>("RPG");
                    break;

                default:
                    DIWrapper.Register<CURRENT.NAMESPACE.IGame, PlatformGame>("Platform");
                    game = DIWrapper.Resolve<IGame>("Platform");
                    break;
            }

            return game;
        }
    }

Here, we create a _Factory_ that registers the "_IGame_" interface with multiple objects and gives us the right object depending of what we ask.

Let's imagine that we have the following code in our "_Main_" method:

    static void Main(string[] args)
    {
        DIWrapper.Register<IGameFactory<IGame>, GameFactory<IGame>>();
        DIWrapper.Register<IInitializer, Initializer>();
        IInitializer initializer = DIWrapper.Resolve<IInitializer>();
    }

We can then have the follwing code:

    public class Initializer : IInitializer
    {
        public Initializer(IGameFactory<IGame> gameFactory)
        {
            gameFactory.Type = GameType.RPG;
            IGame game1 = gameFactory.Create();
            game1.Play();
            gameFactory.Type = GameType.Platform;
            IGame game2 = gameFactory.Create();
            game2.Play();
        }
    }

## With unit tests ##

_Unity_ will help us to write our unit tests by realizing mock objects. Here, we also use the _Moq_ package.

    [TestClass]
    public class InitializerTest
    {
        /*********************/
        /***** ATTRIBUTS *****/
        /*********************/

        /// <param name="_instance">IInitializer object</param>
        /// <param name="_container">_Unity_ Container</param>

        private IInitializer _instance;
        private IUnityContainer _container;

        /**************************************************/
        /**************************************************/

        /****************************/
        /***** INITIALIZE TESTS *****/
        /****************************/

        [TestInitialize] 
        public void InitializeTests()
        {
            _container = new UnityContainer();

            var gameFactoryMock = new Mock<IGameFactory<IGame>>();
            _container.RegisterInstance<IGameFactory<IGame>>(gameFactoryMock.Object);

            _container.RegisterType<IInitializer, Initializer>();
            _instance = _container.Resolve<IInitializer>();
        }

        /**************************************************/
        /**************************************************/

        /**************************/
        /***** CHECK INSTANCE *****/
        /**************************/

        [TestMethod]
        public void CheckInstance()
        {
            Assert.IsNotNull(_instance, "Instance of object Initializer should not be null.");
        }
    }

## Conclusion ##

Through this article, we saw how we can easily achieve _Dependency Injection_ with _Unity_. We saw that we can use various methods to apply this pattern and how we can wrap our container. We also saw how it can help us through our unit tests.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
# Getting Started With Symfony 4 #

Through this article, we are going to take a look at the _Symfony 4_ framework made by _SensioLabs_.

## Introduction ##

To create a web application, we have many tools at our disposal. Choosing is sometimes a hard task. However, some tools are some kind of reference, as _Symfony_ is. Here, we are going to have an overview of this framework. To achieve this, we are going to use the fourth version.

## What is Symfony? ##

_Symfony_ is the leading _PHP_ framework available to everyone under an _Open Source_ license. It is built on top of a set of decoupled and reusable components named _Symfony Components_. _Symfony_ use generic components to allow us to focus on other tasks.

## An overview of some elements ##

Before we dive into the code, let's have an overview of some elements used by _Symfony_ to understand better what we are going to do.

### Symfony Components ###

_Symfony Components_ are a set of decoupled and reusable _PHP libraries_. Those components can even be used without _Symfony_.

### Symfony Flex ###

_Symfony Flex_ is a way to install and manage _Symfony_ applications. It automates the most common tasks of _Symfony_ applications.

It is a _Composer_ plugin that modifies the behavior of the _require_, _update_, and _remove_ commands. For example, when we execute the require command, the application will make a request to the _Symfony Flex_ server before trying to install the package with _Composer_. If there is no information about that package that we want to install, the _Flex_ server returns nothing and the package installation follows the usual procedure based on _Composer_. If there is information, _Flex_ returns it in a file called a "_recipe_" and the application uses it to decide which package to install and which automated tasks to run after the installation.

_Flex_ keeps tracks of the recipes it installed in a _symfony.lock_ file, which must be committed to our code repository.

_Recipes_ are defined in a _manifest.json_ file and the instructions defined in this file are also used by _Flex_ when uninstalling dependencies to undo all changes.

### Security Checker ###

_Security Checker_ is a command-line tool that checks if our application uses dependencies with known security vulnerabilities.

### Doctrine ###

_Symfony_ doesn't provide a component to work with databases. However, it provides an integration of the _Doctrine library_. _Doctrine_ is an _object-relational mapper_ (_ORM_). It sits on top of a powerful database abstraction layer (_DBAL_).

In a few words, _Doctrine_ allows us to _insert_, _update_, _select_ or _delete_ an object in a relational database. It also allows us to generate or update tables via classes.

### Twig ###

_Twig_ is a template engine for _PHP_ and can be used without _Symfony_, although it is also made by _SensioLabs_.

## A few terms ##

Through our example, we are also going to use a few terms. Let's define them before.

### Controller ###

A _Controller_ is a _PHP_ function we create. It reads information from a _Request Object_. It then creates and returns a _Response Object_. That response could be anything, like _HTML_, _JSON_, _XML_ or a file.

### Route ###

A _Route_ is a map from a _URL_ path to a _Controller_. It offers us clean _URLs_ and flexibility.

### Requests and Responses ###

_Symfony_ provides an approach through two classes to interact with the _HTTP request_ and _response_. The _Request class_ is a representation of the _HTTP request message_, while the _Response class_, obviously, is a representation of an _HTTP response_ message.

A way to handle what comes between the _Request_ and the _Response_ is to use a _Front Controller_. This file will handle every request coming into our application. It means it will always be executed and it will manage the routing of different _URLs_ to different parts of our application. 

In _Symfony_, incoming requests are interpreted by the _Routing_ component and passed to _PHP_ functions that return _Response Objects_. It means that the _Front Controller_ will pass the _Request_ to _Symfony_. This last one will create a _Response Object_ and turns it to text headers and content that will finally be sent back.

## Project structure ##

When we will start our project, our project directory will contain the following folders:

* conifg - holds config files
* src - where we place our PHP code
* bin - contains executable files
* var - where automatically-created files are stored (cache, log)
* vendor - contains third-party libraries
* public - contains publicly accessible files

## A simple example ##

Now we know more about _Symfony_, it is time to do something with it.

### Setting up our project ###

Let's first start by creating our project. We can do as _Symfony_'s documention suggets or with, for example, use _PHP Docker Boilerplate_ if we want to use _Docker_. However, we have to be sure that we have at least _PHP 7.1_ and our configuration allows _URL rewriting_. If we are a _macOS_ user, we can encounter some trouble with our _PHP_ version. An explanation of how update our _PHP_ version can be found [here](https://php-osx.liip.ch/). We also have to be sure that we have the latest version of _Composer_.

Following _Symfony_'s documention, it is something like so:

    composer create-project symfony/skeleton simple-app
<small>_Setting up our project_</small>

This creates a new directory named _simple-app_, downloads some dependencies into it and generates the basic directories and files we need to get started.

Now, let's move into our directory to install and run our server:

    cd simple-app
    composer require server --dev
    php bin/console server:run
<small>_Installing and running our server_</small>

Now, if we use _PHP Docker Boilerplate_, it would be like so:

    git clone https://github.com/webdevops/php-docker-boilerplate.git simple-app
    cd simple-app
    cp docker-compose.development.yml docker-compose.yml
    composer create-project symfony/skeleton app
    composer require server --dev
    docker-compose up -d
<small>_Installing Symfony using PHP Docker Boilerplate_</small>

Webserver will be available at port _8000_.

We also have to change some values in _etc/environment*.yml_:

    DOCUMENT_ROOT=/app/public/
    DOCUMENT_INDEX=index.php
<small>_etc/environment*.yml file_</small>

To run the _Symfony CLI_, we can do it like so:

    docker-compose run --rm app php bin/console server:start
    # OR
    docker-compose run --rm app bash
    php bin/console server:start
<small>_Running Symfony CLI using PHP Docker Boilerplate_</small>

When or project is ready, if we want to install _Security Checker_, we have to do it like so:

    composer require sec-checker
<small>_Installing Security Checker_</small>

We also want to install the _Web Debug Toolbar_. It displays debugging information along the bottom of our page while developing.

    composer require --dev profiler
<small>_Installing Web Debug Toolbar_</small>

Maybe changing the permissions for the debugger will be necessary.

    chmod -R 1777 /app/var
<small>_Changing permissions_</small>

### Creating our first page ###

Let's now make our first page. But, first, let's install what we are going to use to define our _Routes_:

    composer require annotations
<small>_Installing Framework Extra Bundle_</small>

This allows us to use _Annotation Routes_ instead of defining them into a _YAML_ file. We also need to install _Twig_:

    composer require twig
<small>_Installing Twig_</small>

We can now create our first template:

    <h1>Hello World!</h1>
<small>_templates/hello.html.twig file_</small>

Now, let's create our first _Controller_:

    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;

    class SimpleController extends Controller
    {
        /**
        * @Route("/")
        */
        public function index()
        {
            return $this->render('hello.html.twig');
        }
    }
<small>_src/Controller/SimpleController.php file_</small>

Now, let's try our newly created page by visiting _http://localhost:8000_.

### Connecting to the database ###

Now, it is time to try to connect our application to a database. Here, we are going to use _MySQL_. First, we have to install _Doctrine_ and the _MakerBundle_.

    composer require doctrine maker
<small>_Installing Doctrine and MakerBundle_</small>

Now, we can edit the _.env_ file like so:

    DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name"
    # If we are using PHP Docker Boilerplate, it will be something like that:
    # DATABASE_URL=mysql://dev:dev@mysql:3306/database
<small>_.env file_</small>

We can now use _Doctrine_ to create the database:

    php bin/console doctrine:database:create
<small>_Creating the database_</small>

### Entity Class and Migrations ###

We are now ready to create our first _Entity Class_. Let's do it like so:

    php bin/console make:entity Post
<small>_Creating a Post Entity_</small>

Each property of an _Entity_ can be mapped to a column in a corresponding table in our database. Using mapping will allow _Doctrine_ to save an _Entity Object_ to the corresponding table. It will also be able to query from that same table and turn the returned data into objects.

Let's now add more fields to our _Post Entity_:

    namespace App\Entity;

    use Doctrine\ORM\Mapping as ORM;

    /**
    * @ORM\Entity(repositoryClass="App\Repository\PostRepository")
    */
    class Post
    {
        /**
        * @ORM\Id
        * @ORM\GeneratedValue
        * @ORM\Column(type="integer")
        */
        private $id;

        /**
        * @ORM\Column(type="string", length=100)
        */
        private $title;

        /**
        * @ORM\Column(type="text")
        */
        private $content;

        /**
        * @ORM\Column(type="text")
        */
        private $content;
    }
<small>_Entity/Post.php file_</small>

Now we are ready to update our database. First, let's create a _migration_:

    php bin/console doctrine:migrations:diff
<small>_Creating a migration_</small>

And now, we can execute our newly generated _migration_:

    php bin/console doctrine:migrations:migrate
<small>_Running our migration_</small>

We now need to create _public_ _setters_ and _getters_ for our properties:

    ...
    public function getId()
    {
        return $this->id;
    }

    public function getTitle()
    {
        return $this->title;
    }

    public function setTitle($title)
    {
        $this->title = $title;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function setContent($content)
    {
        $this->content = $content;
    }

    public function getExcerpt()
    {
        return $this->excerpt;
    }

    public function setExcerpt($excerpt)
    {
        $this->excerpt = $excerpt;
    }
    ...
<small>_Entity/Post.php file edited_</small>

We can now create a corresponding _Controller_ like so:

    php bin/console make:controller PostController
<small>_Creating a Controller_</small>

Let's edit our _Controller_ to have something like so:

    namespace App\Controller;

    use App\Entity\Post;
    use App\Repository\PostRepository;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;

    class PostController extends Controller
    {
        /**
        * @Route("/posts", name="post")
        */
        public function index(): Response
        {
            $posts = $this->getDoctrine()
                        ->getRepository(Post::class)
                        ->findAll();
            return $this->render('posts/list.html.twig', ['posts' => $posts]);
        }
    }
<small>_Controller/PostController.php file edited_</small>

As we can see in the above code, we query our posts before we pass the result to a view. To get our items, we use what is called a _Repository_. This last one is a _PHP class_ that helps us to fetch entities of a certain class. We can edit this _Repository_ class if we want, so we can add methods for more complex queries into it.

We can now edit the _base.html.twig_ template and create a new named _list.html.twig_ in a new subdirectory called posts.

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>{% block title %}Simple App{% endblock %}</title>
            {% block stylesheets %}{% endblock %}
        </head>
        <body>
            {% block body %}{% endblock %}
            {% block javascripts %}{% endblock %}
        </body>
    </html>
<small>_templates/base.html.twig file_</small>

    {% extends 'base.html.twig' %}

    {% block body %}
        <h1>Posts</h1>

        <table>
            <thead>
                <tr>
                    <th scope="col">Title</th>
                    <th scope="col">Actions</th>
                </tr>
            </thead>
            <tbody>
            {% for post in posts %}
                <tr>
                    <td>{{ post.title }}</td>
                    <td>
                        <div class="item-actions">
                            <a href="">
                                See
                            </a>
                            <a href="">
                                Edit
                            </a>
                            <a href="">
                                Delete
                            </a>
                        </div>
                    </td>
                </tr>
            {% else %}
                <tr>
                    <td colspan="4" align="center">No posts found</td>
            </tr>
            {% endfor %}
            </tbody>
        </table>
    {% endblock %}
<small>_templates/posts/list.html.twig file_</small>

Now, if we go to _localhost:8000/posts_, we will see a pretty rough interface and our empty posts list.

To fill our posts list, we are going to create a form. Let's install a new component:

    composer require form
<small>_Installing Form component_</small>

And of course, we need to validate that form. We can make it with _Validator_:

    composer require validator
<small>_Installing Validatior_</small>

We can now create a template for our form:

    {% extends 'base.html.twig' %}

    {% block body %}
        <h1>New post</h1>

        {{ form_start(form) }}
        {{ form_widget(form) }}
        {{ form_end(form) }}

        <a href="{{ path('posts') }}">Back</a>

    {% endblock %}
<small>_templates/posts/new.html.twig_</small>

Here, we create the template that is used to render the form. The _form start(form)_ renders the start tag of the form while the _form end(form)_ renders the end tag of the form. _form widget(form)_ renders all the fields, which includes the field element itself, a label and any validation error messages for the field. It is also possible to render each field manually as described in the [Symfony documentation](https://symfony.com/doc/current/form/rendering.html).

We also need to edit our _Post Entity_:

    namespace App\Entity;

    use Doctrine\ORM\Mapping as ORM;
    use Symfony\Component\Validator\Constraints as Assert;

    /**
    * @ORM\Entity(repositoryClass="App\Repository\PostRepository")
    */
    class Post
    {
        /**
        * @ORM\Id
        * @ORM\GeneratedValue
        * @ORM\Column(type="integer")
        */
        private $id;

        /**
        * @ORM\Column(type="string", length=100)
        * @Assert\NotBlank()
        */
        private $title;

        /**
        * @ORM\Column(type="text")
        * @Assert\NotBlank()
        */
        private $content;

        /**
        * @ORM\Column(type="text")
        * @Assert\NotBlank()
        */
        private $excerpt;
        ...
    }
<small>_Entity/Post.php file edited_</small>

Validation is done by adding a set of rules, or constraints, to a class. A completed documentation about those different rules can be found [here](https://symfony.com/doc/current/validation.html). In _Symfony_, validation is applied to the underlying object, it means it is checked if the object, here _Post_, is valid after the form has applied the submitted data to it.

Now, edit our _PostController_ like so:

    namespace App\Controller;

    use App\Entity\Post;
    use App\Repository\PostRepository;
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Security;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Form\Extension\Core\Type\TextType;
    use Symfony\Component\Form\Extension\Core\Type\TextareaType;
    use Symfony\Component\Form\Extension\Core\Type\SubmitType;

    class PostController extends Controller
    {
        /**
        * @Route("/posts", name="posts")
        */
        public function index(PostRepository $repository): Response
        {
            $posts = $this->getDoctrine()
                        ->getRepository(Post::class)
                        ->findAll();
            return $this->render('posts/list.html.twig', ['posts' => $posts]);
        }

        /**
        * @Route("/posts/new", name="new")
        * @Method({"GET", "POST"})
        */
        public function new(Request $request)
        {
            $post = new Post();

            $form = $this->createFormBuilder($post)
                        ->add('title', TextType::class)
                        ->add('content', TextareaType::class)
                        ->add('excerpt', TextareaType::class)
                        ->add('create', SubmitType::class)
                        ->getForm();

            $form->handleRequest($request);

            if ($form->isSubmitted() && $form->isValid()) {

                $em = $this->getDoctrine()->getManager();
                $em->persist($post);
                $em->flush();

                $this->addFlash('success', 'post created');

                return $this->redirectToRoute('posts');
            }

            return $this->render('posts/new.html.twig', [
                'form' => $form->createView(),
            ]);
        }
    }
<small>_Controller/PostController.php file edited_</small>

In the first part of our new method, we use the _Form Builder_. We add three fields, corresponding to the properties of the _Post class_ and a submit button.

We then call _handleRequest_ to see if the form was submitted or not when the page is loading. If the form was submitted and if it is valid, we can perform some actions using the _Post Object_.

As we can see, here we use the _persist_ method that tells _Doctrine_ to "manage" the _Post Object_. We then call the _flush_ method that tells _Doctrine_ to look through all of the objects that it's managing to see if they need to be persisted to the database.

We then render the view. It is important that the _createView_ method is placed after the _handleRequest_ method. Otherwise, changes done in the _* SUBMIT_ events aren't applied to the view.

Now, with what know, we can go a little further and add some features to our application. First, let's edit our _PostController_ like so:

    namespace App\Controller;

    use App\Entity\Post;
    use App\Repository\PostRepository;
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Security;
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Form\Extension\Core\Type\TextType;
    use Symfony\Component\Form\Extension\Core\Type\TextareaType;
    use Symfony\Component\Form\Extension\Core\Type\SubmitType;

    class PostController extends Controller
    {
        /**
        * @Route("/posts", name="posts")
        */
        public function index(PostRepository $repository): Response
        {
            $posts = $this->getDoctrine()
                        ->getRepository(Post::class)
                        ->findAll();
            return $this->render('posts/list.html.twig', ['posts' => $posts]);
        }

        /**
        * @Route("/posts/new", name="new")
        * @Method({"GET", "POST"})
        */
        public function new(Request $request)
        {
            $post = new Post();

            $form = $this->createFormBuilder($post)
                        ->add('title', TextType::class)
                        ->add('content', TextareaType::class)
                        ->add('excerpt', TextareaType::class)
                        ->add('create', SubmitType::class)
                        ->getForm();

            $form->handleRequest($request);

            if ($form->isSubmitted() && $form->isValid()) {

                $em = $this->getDoctrine()->getManager();
                    
                $em->persist($post);
                $em->flush();

                $this->addFlash('success', 'post created');

                return $this->redirectToRoute('posts');
            }

            return $this->render('posts/new.html.twig', [
                'form' => $form->createView(),
            ]);
        }

        /**
        * @Route("/{id}/show", requirements={"id": "\d+"}, name="show")
        * @Method("GET")
        */
        public function show(Post $post): Response
        {
            return $this->render('posts/show.html.twig', [
                'post' => $post,
            ]);
        }

        /**
        * @Route("/{id}/edit", requirements={"id": "\d+"}, name="edit")
        * @Method({"GET", "POST"})
        */
        public function edit(Request $request, Post $post): Response
        {
            $form = $this->createFormBuilder($post)
                        ->add('title', TextType::class)
                        ->add('content', TextareaType::class)
                        ->add('excerpt', TextareaType::class)
                        ->add('edit', SubmitType::class)
                        ->getForm();

            $form->handleRequest($request);

            if ($form->isSubmitted() && $form->isValid()) {

                $em = $this->getDoctrine()->getManager();
                    
                $em->flush();

                $this->addFlash('success', 'post edited');

                return $this->redirectToRoute('posts');
            }

            return $this->render('posts/edit.html.twig', [
                'post' => $post,
                'form' => $form->createView(),
            ]);
        }

        /**
        * @Route("/{id}/delete", requirements={"id": "\d+"}, name="delete")
        * @Method({"GET"})
        */
        public function delete(Request $request, Post $post): Response
        {
            $em = $this->getDoctrine()->getManager();

            $em ->remove($post);
            $em ->flush();

            $this->addFlash('success', 'post deleted');

            return $this->redirectToRoute('posts');
        }
    }
<small>_src/Controller/SimpleController.php file edited_</small>

We can now create two additional templates:

    {% extends 'base.html.twig' %}

    {% block body %}
        <h1>{{ post.title }}</h1>

        {{ post.content }}

        <a href="{{ path('posts') }}">Back</a>

    {% endblock %}
<small>_templates/posts/show.html.twig file_</small>

    {% extends 'base.html.twig' %}

    {% block body %}
        <h1>Edit post</h1>

        {{ form_start(form) }}
        {{ form_widget(form) }}
        {{ form_end(form) }}

        <a href="{{ path('posts') }}">Back</a>

    {% endblock %}
<small>_templates/posts/edit.html.twig file_</small>

Now, we can read, edit our delete the posted we have created with our application.

## Conclusion ##

Through this article we took a look at _Symfony 4_. We had an overview of the different concepts it is based on. We set up an installation of _Symfony 4_ and created a very simple application that let us interact with a _MySQL_ database.

Now, we still have many things to see, like _Security Annotations_ our custom _Twig Filters_ that allow us to build a better application.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
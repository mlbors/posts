# Docker and Laravel #

In this post, we are going to set up a _Laravel_ project using _Docker_ and _Laradock_.

_Docker_ is an open-source platform that enables a _Linux_ application and its dependencies to be packaged as a container. Container-based virtualization isolates applications from each other on a _shared operating system_. Because they share the same _OS_, containers are portable among different _Linux distributions_ and are smaller than _virtual machine_ (VM) images.

The first thing we have to do is to download and install _[Docker](https://www.docker.com)_. When it is done, we can create a folder where ever we want. In this folder, to install _Laradock_, we have to do the following command:
    
    
    git clone https://github.com/laradock/laradock.git

_Cloning Laradock_

Now, we can do the following things:
    
    
    cd laradock
    cp env-example .env

_Moving into Laradock’s folder and creating the .env file_

We are now going to build, create and start the containers:
    
    
    docker-compose up -d nginx mysql

_Building and starting the containers_

Here, we are running the containers in detach mode saying we want to use _nginx_ and _MySQL_.

We are now ready to install _Laravel_. First, we have to connect to the _Workspace_ container. This container will let us install tools for our development.
    
    
    docker-compose exec workspace bash

_Entering the Workspace container_

We can now initialize our project:
    
    
    composer create-project laravel/laravel quickstart --prefer-dist

_Installing Laravel using Composer_

We can exit our container:
    
    
    exit

_Leaving the container_

Now, because _Laradock_ assumes that it is by default in the same folder than our _Laravel_ project, if we go to _http://localhost_, we will see an error. We can solve that by moving our “quickstart” folder into the “laradock” folder or by changing the _docker-compose.yml_ file like so:
    
    
    ### Applications Code Container #############################
    
        applications:
          image: tianon/true
          volumes:
            - ../quickstart/:/var/www

_docker-compose.yml file_

Now, because we have made changes in the _docker-compose.yml_ file, we have to stop and run it again:
    
    
    docker-compose stop
    docker-compose up -d nginx mysql

_Rebuilding_

Now, if we go to _http://localhost_, we will see the _Laravel’s_ welcome page.

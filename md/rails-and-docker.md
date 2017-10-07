## [Rails and Docker](https://mlbors.tumblr.com/post/160836332260/rails-and-docker)

In this post, we are going to set up a _Ruby on Rails_ project using _Docker_.

_Docker_ is an open-source platform that enables a _Linux_ application and its dependencies to be packaged as a container. Container-based virtualization isolates applications from each other on a _shared operating system_. Because they share the same _OS_, containers are portable among different _Linux distributions_ and are smaller than _virtual machine_ (VM) images.

The first thing we have to do is to download and install _[Docker](https://www.docker.com)_. When it is done, we can create a folder where ever we want. In this folder, we are firstly going to create the _Gemfile_ and the _Gemfile.lock_ files like so:
    
    
    source 'https://rubygems.org'
    
    gem 'rails'
    gem 'pg'

_Gemfile_
    
    
    _empty_

_Gemfile.lock_

Then, we are going to create the docker-compose.yml file: 
    
    
    version: '2'
    
    services:
      app:
        build: .
        command: bundle exec rails s -p 3000 -b '0.0.0.0'
        volumes:
          - .:/app
        depends_on:
          - db
        links:
          - db
        ports:
          - "3000:3000"
    
      db:
        image: postgres:9.5
        restart: always
        volumes:
          - db_data:/var/lib/postgresql
        ports:
          - "5432:5432"
        logging:
          driver: none
    
    volumes:
      db_data:
        driver: local

_docker-compose.yml file_

Here, we create our main container, called “app”, map the ports and link this container to the one called “db”. This last one is responsible of the database. In this part of the file, we also set a volume for the database to ensure the data’s persistence. We can now create the _Dockerfile_:
    
    
    FROM ruby:2.4.1
    
    RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
    
    RUN mkdir /app
    WORKDIR /app
    
    ADD Gemfile /app/Gemfile
    ADD Gemfile.lock /app/Gemfile.lock
    
    RUN bundle install
    
    ADD . /app

_Dockerfile_

Now, we are going to initialize our application and build the _containers_:
    
    
    docker-compose run app rails new . --force --database=postgresql --skip-bundle
    docker-compose build
    docker-compose up -d

_Creating the app_

To make things work, we have to change a few things in the _database.yml_ file:
    
    
    -----
    
    development:
      <<: *default
      database: app_development
      username: postgres
      password:
      host: db
      port: 5432
    
    -----

_databse.yml file_

To make things a little more funny, we are going to create a _scaffold_. A _scaffold_ in _Rails_ is a full set of model, database migration for that model, controller to manipulate it, views to view and manipulate the data, and a test suite for each of the above.
    
    
    docker-compose run --rm app rails g scaffold post title body:text

_Scaffolding_

Let’s create our database and a migration:
    
    
    docker-compose run app rake db:create
    docker-compose run --rm app rake db:migrate

_Database creation and migration_

Now, if we go to _http://localhost:3000_, we will see the Rails homepage and if we go to_ http://localhost:3000/posts_, we will see a simply interface that let us play with _CRUD operations_.

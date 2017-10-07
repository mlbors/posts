# Setting up Sage 9 with Docker #

_Sage_ is a _WordPress starter theme_ developed by _Roots_. Let's see how we can quickly set it up with _Docker_.

To achieve our goal, we are going to use the _PHP Docker Boilerplate_ from _WebDevOps_. We can clone the repository by simply use:

	git clone https://github.com/webdevops/php-docker-boilerplate.git wordpress-project
	cd wordpress-project
	cp docker-compose.development.yml docker-compose.yml

_Setting up the environment_

If we want to use _phpMyAdmin_, we can remove comments at the following lines at the end of the _docker-compose.yml_ file:

	phpmyadmin:
		image: phpmyadmin/phpmyadmin
		links:
		  - mysql
		environment:
		  - PMA_HOSTS=mysql
		  - VIRTUAL_HOST=pma.boilerplate.docker
		  - VIRTUAL_PORT=80
		volumes:
		  - /sessions
		ports:
		  - "8001:80"
		 
_docker-compose.yml file_

Now, we can simply do the following command:

	docker-compose up -d

_Running our containers_

For the database, we can use the following information:

	DB_NAME: database
	DB_USER: dev
	DB_PASSWORD: dev
	DB_HOST: mysql:3306

_Database information_

Now, in our directory _wp-content/themes_, we can do the following thing:

	composer create-project roots/sage sage dev-master
	cd sage
	yarn
	yarn run build

_Theme installation_

We just have to do one last thing: in the _resources/assets/config.json_ file, we have to change the following lines:

	...
	"publicPath": "/wp-content/themes/sage",
	"devUrl": "http://localhost:8000",
	...

_resources/assets/config.json file_

Now, if the theme is activated in _wp-admin_, we just have to run the following command:

	yarn run start

_Starting Browsersync session_

Now, our assets will be compiled when file changes are made and our site will be visible at _localhost:3000_.

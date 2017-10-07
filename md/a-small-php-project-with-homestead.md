# A small PHP project with Homestead #

In this post, we are going to initialize a simple _PHP_ project with _Laravel Homestead_.

_Laravel Homestead_ is a pre-packaged _Vagrant box_ that provides a development environment without requiring us to install _PHP_, a web server or any other server software on our local machine.

The first thing we have to do is to download and install [VirutalBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/). When it is done, we can add the _laravel/homestead box_ to our _Vagrant_ installation using the following command in our terminal:
    
    
    vagrant box add laravel/homestead

_Add Homestead box to Vagrant_

Now, let’s go to our “home” directory and install _Homestead_ using _Git_:
    
    
    cd ~
    git clone https://github.com/laravel/homestead.git Homestead

_Clone Homestead_

Once the _Homestead_ directory is cloned, we can go into that last one and initialize _Homestead_:
    
    
    cd Homestead
    bash init.sh

_Initialize Homestead_

Now we have to configure our installation by opening the _Homstead.yaml_ file in our directory. First, we need to set the _provider_:
    
    
    provider: virtualbox

_Provider section in Homestead.yaml_

We may have to generate our _SSH key_ by doing the following command:
    
    
    ssh-keygen -t rsa

_Generate SSH key_

By default, our key will be saved in our “home” directory.

Let’s now set the _folders_ property. That last one lists all of the folders we want to share with our _Homestead_ environment. As files within these folders are changed, they will be kept in sync between our local machine and the _Homestead_ environment.
    
    
    folders:
        - map: ~/Code/Php
          to: /home/vagrant/Code

_Folders section in Homestead.yaml_

We are now going to map our sites:
    
    
    sites:
        - map: our-project-name.dev
          to: /home/vagrant/Code/our-project-name.dev/
        - map: second-project.dev
          to: /home/vagrant/Code/second-project.dev/web
          type: symfony

_Sites section in Homestead.yaml_

We need to set a few things in our _hosts_ file. On _macOS_ and _Linux_, this file is located at _/etc/hosts_. On _Windows_, it is located at _C:\Windows\System32\drivers\etc\hosts_. Let’s add the following lines:
    
    
    # Vagrant
    192.168.10.10 our-project-name.dev
    192.168.10.10 second-project.dev

_Hosts file_

We have to make sure that the IP address listed is the one set in our _Homestead.yaml_ file.

We can run our _box_ by doing the folliwng command:
    
    
    vagrant up —-provision

_Provision and run the box_

If we make any change in the _Homestead.yaml_ file, we need to update the _Nginx_ configuration on the _virtual machine_ with the following command:
    
    
    vagrant reload --provision

_Reload the box_

If everything is alright, we can now access to _our-project-name.dev_ and _second-project.dev_ via our web browser.

We may also need to connect to our machine to install some stuff. If so, we can do it like so from our _Homestead_ directory:
    
    
    vagrant ssh

_Connection using SSH_

A _homestead database_ is configured for both _MySQL_ and _Postgres_ out of the box. To connect to our _MySQL_ or _Postgres_ database from our host machine’s database client, we have to connect to 127.0.0.1 and port 33060 (MySQL) or 54320 (Postgres). For both databases, the username is _homestead_ and the password is _secret_.

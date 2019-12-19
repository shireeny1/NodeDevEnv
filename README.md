## Download and install Vagrant 64 bit and Virtualbox 6.0
- On bash command line, cd into your file and run ``vagrant init`` - this creates a vagrant file
- Run ``atom .`` - this opens the vagrant file on atom
- Set:
  - config.vm.box = "ubuntu/xenial64"
  - config.vm.network "private_network", ip: "192.168.10.100"
  - config.hostsupdater.aliases = ["development.local"]
- ``vagrant up`` - spins up the machine
- ``vagrant ssh`` - enters the vm (makes connection to the vm)
  - run ``sudo apt-get update`` - updates packages
- In order to install nginx, inside the vm run the command:
  - ``sudo apt-get install nginx``
- To start up nginx:
  - ``sudo systemctl start nginx``

- Machine needs to be provisioned, this means to make a bash script to run on vagrant so ``sudo apt-get update`` and ``sudo apt-get install nginx`` and other commands are automatically run at the start
  - Locate your '/environment' file on git bash and run the command ``touch provision.sh' then 'atom .``
  - When you 'ls' you should see the files inside '/environment', these files will be 'spec-tests' and your 'provision.sh' file that should be highlighted green with an *

- Once your provision.sh file is open in atom, write the following:
  - ``#!/bin/bash

      sudo apt-get update -y
      sudo apt-get upgrade -y
      sudo apt-get install nginx -y

      sudo systemctl start nginx

      sudo apt-get install nodejs -y
      curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
      sudo apt-get install -y nodejs

      sudo npm install pm2 -g``

- On your vagrant file you will need to sync a folder:
  - config.vm.synced_folder "app", "/app"
    - "app" is the folder to sync
    - "/app" is the folder to sync on the vm
- Now you have also provisioned your machine you will need to add this onto your vagrant file:
  - config.vm.provision "shell", path: "environment/provision.sh"

### Installing Ruby and Bundler
- Install ruby from rubyinstaller.org
- Once ruby has been installed, restart your terminal and locate yourself back into ''/environment'
- Install bundler by running the command:
  - ``gem install bundler``
  - Terminal requires restarting again
- cd into your ''/environment' location
- A file name 'spec-tests' should be inside the file '/environment', cd into 'spec-tests'
- Run the command ``bundle``
- Then run the command ``rake spec`` - this runs all the test (all should pass)
- Run ``vagrant destroy`` and then ``vagrant up``

### Clone the app
- Move to the correct location where your synced folder is, in this case it's in 'app' which is inside 'starter-code'
- Run ``vagrant ssh`` to enter the vm
- Now inside, you will have to move to the correct location of the synced folder inside the vm, in this case it's in '/app'
- Run the following commands:
  - ``cd ..``
  - ``ls`` - make sure that 'app' is there
  - `` cd app``
  - ``npm install`` - this installs dependencies from the .json file
  - ``npm start`` - this loads/runs the .js file
  - You will then see the message ``Your app is ready and listening on port 3000``
- Whilst still inside the vm, go on Google Chrome and type in 'development.local:3000' in the search bar and you should see the homepage displaying a Sparta logo and a message

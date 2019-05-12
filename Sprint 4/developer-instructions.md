## Augur Installation on local machine

[Click here for instructions](https://github.com/computationalmystic/augur-group26/blob/master/README.md)

Please follow the instructions at the link above, except when you clone the repository to 
your local machine, make sure and use 

1. Clone our Group's (26) repository and boot up the VM.

```bash
git clone https://github.com/computationalmystic/augur-group26.git
cd augur-group26
make vagrant
```

Note: you'll probably see a fair bit of errors during this provisioning process as Augur is getting installed. Don't worry about them, most of them are harmless. *Probably.*

2. Log in as `root` and navigate to `/vagrant/augur`. This folder is synced with your local clone of `augur`, meaning you'll be able to use your preferred local editor and just use the VM to run augur.  
```bash
# inside the vagrant VM
sudo su -
cd /vagrant/augur

# due to vagrant weirdness, we have to manually install the python packagew (this might take a while)
$AUGUR_PIP install --upgrade .
```

3. Augur.config.json 
   `augur.config.json` contains several elements that crucial for Augur. First, you need to add your GitHub API key under the `GitHub` section. Finally, you will need to provide log in credientals for the Facade and Ghtorrent databases.

4. Starting the backend and frontend servers
** There are 3 ways to start the servers. `make dev` will start the servers but they will stop will the terminal is closed or the user hits Command + C. `make dev-start`is similar to `make dev` however pressing Command + C will not stop the process. If you use `make dev-start`, you will need to use `make dev-stop` to kill the processes. You can also use `nohup make dev-start` to run launch the servers indefinitely to stop the servers use `make dev-stop`.

```bash
make dev
#Start                Stop
make dev-start       make dev-stop
nohup make dev-start make dev-stop
```

5. When you're done working in the VM, type `exit` twice: once to log out of `root`, and another to log out of the VM. Don't forget to shut down the VM with `vagrant halt`.

If you're interested in adding a new plugin, data source, or metric, check out the [backend development guide](http://augur.augurlabs.io/static/docs/dev-guide/3-backend.html). If new visualizations are more your speed, you'll want the [frontend development guide](http://augur.augurlabs.io/static/docs/dev-guide/4-frontend.html\).

### TL;DR

```bash
# on your local machine

# using your Git client: 

git clone https://github.com/computationalmystic/augur-group26.git

# using Command Prompt
cd augur-group26
vagrant up
vagrant ssh

# inside the vagrant VM
sudo su -
cd /vagrant/augur

# due to vagrant weirdness, we have to manually install the python packages
$AUGUR_PIP install --upgrade .

# add your GitHub personal access token to augur.config.json

# start the frontend and backend servers
make dev
# full steam ahead!
```

instead of the repository in the given example.

From there, when you launch augur on your vagrant instance it should show 
our version of augur. 


## EC2 INSTANCE VIA AMAZON

### Open EC2 Inbound Ports

The first thing you will need to do is open the inbound 3333 and 5000 to your EC2 instance. If you need help the following link will will walk you through this step by step.

[https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/ec2%20add%20protocols.md](https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/ec2%20add%20protocols.md)

### Install NodeSource

curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -

### Install NodeJS

sudo yum install -y nodejs

### Install Anaconda
curl https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh > Anaconda.sh

chmod +x Anaconda.sh

### You must agree to Anaconda's license terms to proceed
./Anaconda.sh -b

rm Anaconda.sh

### You may need to install git

sudo yum install git

### You will need a clone of your repo

git clone https://github.com/computationalmystic/augur-group26.git

### Switch directories to augur-group26

cd augur-group26

### Edit augur.config.json to use http://classes.augurlabs.io/

vim augur.config.json

Using the following link as a template edit augur.config.json to run the server http://classes.augurlabs.io/

[https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/Jason%20outline](https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/Jason%20outline)

### to install dev

make install-dev

### To launch you will need to run

make dev-start

make dev
 
### To lauch and run indefinely 

nohup make dev-start

### You may have issues with permissions

In case you have problems with ownership or permissions here are the commands you may need to change the EC2 ownerships and permissions

sudo chown -R ec2-user node_modules/
sudo chgrp -R ec2-user node_modules/


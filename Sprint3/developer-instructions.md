## Augur Installation on local machine

[Click here for instructions](https://github.com/computationalmystic/augur-group26/blob/master/README.md)

Please follow the instructions at the link above, except when you clone the repository to 
your local machine, make sure and use 
```bash
git clone https://github.com/computationalmystic/augur-group26.git
cd augur
make vagrant
```
instead of the repository in the given example.

From there, when you launch augur on your vagrant instance it should show 
our version of augur. 


## EC2 INSTANCE VIA AMAZON


### Install NodeSource

curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -
sudo yum install -y nodejs

### Install NodeJS

sudo yum install -y nodejs


### Edit augur.config.json to use http://classes.augurlabs.io/

Using the following link as a template edit augur.config.json to run the server http://classes.augurlabs.io/

[https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/Jason%20outline](https://github.com/computationalmystic/augur-group26/blob/master/Sprint3/Jason%20outline)

### Install Anaconda
curl https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh > Anaconda.sh
chmod +x Anaconda.sh

### You must agree to Anaconda's license terms to proceed
./Anaconda.sh -b
rm Anaconda.sh

### To launch you will need to run

make dev-start
make dev
 
### To lauch and run indefinely 

nohup make dev-start

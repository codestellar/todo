# AWS Workshop Day 1

* Create a static ip
* Download the .pem based SSH Key
* Get the launch script (This is optional)

````shell
#! /bin/bash
#! /bin/bash
sudo /opt/bitnami/ctlscript.sh stop apache
sudo mv /opt/bitnami/apache2/scripts/ctl.sh /opt/bitnami/apache2/scripts/ctl.sh.disabled

cd /home/bitnami
sudo git clone https://github.com/codestellar/todo
cd /home/bitnami/todo
sudo npm install --production

sudo cat << EOF >> /home/bitnami/todo/.env
PORT=80
DB_URL=mongodb://tasks:tasks@localhost:27017/?authMechanism=SCRAM-SHA-1&authSource=tasks
EOF
````

* Create a MEAN stack based machine
* Convert the .pem to .ppk using putty in WINDOWS
* Add Mongo User

````shell
mongo admin --username root -p $(cat ./bitnami_application_password)  

// Create DB
use tasks

// Create User
db.createUser(
    {
        user: "tasks",
        pwd: "tasks",
        roles: [ "dbOwner" ]
    }
)
exit
````

* Goto application 

````shell
cd ~/todo
// Execute
sudo DEBUG=app:* ./bin/www
````

# AWS Workshop Day 1

1. Create a static ip
2. Download the .pem based SSH Key
3. Get the launch script (This is optional)

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

4. Create a MEAN stack based machine
5. Convert the .pem to .ppk using putty in WINDOWS
6. Add Mongo User

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

7. Goto application 

````shell
cd ~/todo
// Execute
sudo DEBUG=app:* ./bin/www
````

[AWS Lightsail - Session 2](lightsail-session-2.md)
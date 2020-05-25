# AWS Lightsail Session 2

* Create a simple ubuntu machine with the following launch script

````shell
#!/bin/bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org

sudo cat /etc/mongod.conf | sed "s/\b127.0.0.1\b/&,$(hostname -i)/" >> /etc/mongod.conf.new

sudo mv /etc/mongod.conf /etc/mongd.conf.old
sudo mv /etc/mongod.conf.new /etc/mongod.conf

sudo service mongod start
````

* Ensure that mongo is running

````
mongo --host $(hostname -i)
````


* setup first node front end server 1

````shell
#!/bin/bash

sudo /opt/bitnami/ctlscript.sh stop apache
sudo mv /opt/bitnami/apache2/scripts/ctl.sh /opt/bitnami/apache2/scripts/ctl.sh.disabled

cd /home/bitnami

sudo git clone https://github.com/mikegcoleman/todo

cd /home/bitnami/todo

sudo npm install --production
sudo npm install pm2@latest -g
````

* Add mongo private ip in environment variables of Front End Node Server 1

````shell
IP=<private IP of mongo>
````

* cd todo
* Set the IP to mongo URL in environment file of node app

````shell
sudo sh -c "cat > ./.env"  << EOF
PORT=80
DB_URL=mongodb://$IP:27017/
EOF
````

* Configure PM2 for use with ubuntu

````shell
sudo pm2 startup ubuntu
````

* start app using PM2

````shell
sudo pm2 start /home/bitnami/todo/bin/www
````

* Allow PM2 to start application on boot

````shell
sudo pm2 save
````

* To view logs

````shell
sudo pm2 logs www
````
Static Apache Webserver Configuration
=====================================

Ready to use static apache2 web server docker-compose configuration 
for a traefik web proxy. With a ssh server with rsync to remotely 
connect to the service and upload the static html files.

This configuration uses the official apache2 docker image:

* apache2 image on docker hub: https://hub.docker.com/_/httpd

This configuration uses the `panubo/sshd` docker image:

* image on docker hub: https://hub.docker.com/r/panubo/sshd/
* github repository of the image: https://github.com/panubo/docker-sshd


Install
-------

```
# clone repository
git clone https://gitlab.wachter-jud.net/docker/apache-static-sshd.git

# move into directory
cd apache-static-sshd

# create directories
## create www directory where your static web page files go
mkdir www
## create folder for ssh keys
mkdir keys

# copy and edit the sample configuration file
cp docker-compose.yml.sample docker-compose.yml

# copy the sample authorized_keys file and put in the public keys
# of the machines that shall connect to it.
cp authorized_keys.sample authorized_keys
```

Edit docker-compose.yml and change the following value:

* `your.web.domain` the domain name under which this web server will be available


Edit the authorized_keys file and put in the public keys 
of all the machines you want to connect to.


Connect & Synchronize via SSH
-----------------------------

Put all files into the `www` directory.

```
# connect with ssh to the server on port 2222
ssh admin@your.domain -p 2222

# copy the files in your local folder to the web server on port 2222
scp -P 2222 -r * admin@your.domain:/home/admin

# synchronize the files in your local folder to the web server on port 2222
rsync -azhe "ssh -p 2222" ./ admin@your.domain:/home/admin
```


Usage
-----

```
# start docker containers
docker-compose up -d

# stop docker containers
docker-compose down

# upgrade container
docker-compose pull
```




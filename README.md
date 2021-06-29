# Static Apache Webserver Configuration

Ready to use static apache2 web server docker-compose configuration 
for a traefik 2 web proxy. With a ssh server with rsync to remotely 
connect to the service and upload the static html files.

This configuration uses the official apache2 docker image:

* apache2 image on docker hub: https://hub.docker.com/_/httpd

This configuration uses the `panubo/sshd` docker image:

* image on docker hub: https://hub.docker.com/r/panubo/sshd/
* github repository of the image: https://github.com/panubo/docker-sshd


## Install

```
# clone repository
git clone https://gitlab.wachter-jud.net/docker/apache-static-sshd.git

# move into directory
cd apache-static-sshd

# copy and edit the sample environment file
cp sample.env .env

## open the .env file in a text editor and configure all
## values to your needs: DOMAIN, ROUTERNAME, SSH_PORT

# copy the sample authorized_keys file and put in the public keys
# of the machines that shall connect to it.
cp authorized_keys.sample authorized_keys

## open the authorized_keys file and put in the public keys 
## of the users that shall be able to connect to it.
## A password is not needed, users are authorized by their public key.
```


## Usage

```
# start docker containers
docker-compose up -d

# stop docker containers
docker-compose down

# upgrade container
docker-compose pull
```


## Connect & Synchronize via SSH

Put all files into the `www` directory.

```
# connect with ssh to the server on port 2222
ssh admin@your.domain -p 2222

# copy the files in your local folder to the web server on port 2222
scp -P 2222 -r * admin@your.domain:/home/admin

# synchronize the files in your local folder to the web server on port 2222
rsync -azhe "ssh -p 2222" ./ admin@your.domain:/home/admin
```


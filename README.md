# Docker Matrix Bootstrap for Synology

This project consists of a script to bootstrap a Matrix Synapse Server along with a Postgresql Server based on Docker Containers.
It is specificly designed to work on a Synology NAS.

## Precondition
The script expects a (Lets Encrypt) certificate registered to the `SYNAPSE_SERVER_NAME` in Synology's certificate manager.

## How to use
1. clone git project
2. edit variables in run.sh
3. execute `sudo ./run.sh prepare`
4. execute `./run.sh up -d`
5. register user
- from cli: `docker exec -ti matrix_app_1  register_new_matrix_user -c /data/homeserver.yaml http://localhost:8008`
- from https://app.element.io/#/register (make sure to switch to your server before login!)
6. login user: https://app.element.io/#/login (make sure to switch to your server before login!)

`run.sh` wraps calls to docker-compose, by rendering the variables into the docker-compose.template on the fly and uses the result with docker-compose. The `run.sh` script passes all options and parameters to docker-compose... Thus, whatever works with docker-compose directly, does work with it as well.

## What does the prepare step do?
Basicly it creates and configures everything required to run the Matrix Synapse server:
1. It creates data folders for Matrix Synapse and Postgresql and fixes file permissions
2. It generate a homeserver.yml, gathers instance unique information from it and renders an opionated homeserver.yml
3. It create a domain specific log configuration for Matrix Synapse and uses it in homeserver.yml
4. It creates a reverse-proxy configuration

Though, it does not start the containers... this is done by executing `./run.sh up -d`

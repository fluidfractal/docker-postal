# docker-postal
[![Build Status](https://iloveyatoo.semaphoreci.com/badges/docker-postal/branches/master.svg)](https://iloveyatoo.semaphoreci.com/projects/docker-postal) [![](https://img.shields.io/badge/image-quay.io%2Filoveyatoo%2Fpostal-blue?style=flat-square&logo=Docker)](https://quay.io/repository/iloveyatoo/postal?tab=tags) [![](https://img.shields.io/badge/image-docker.io%2Filoveyatoo%2Fpostal-blue?style=flat-square&logo=Docker)](https://hub.docker.com/repository/docker/iloveyatoo/postal/tags?page=1&ordering=last_updated)

This project runs daily docker builds for postalhq/postal

**Note that builds have recently been moved to a new account and docker-compose.yml files have been updated accordingly. Please check the updates, especially if you are on the old `ubuntu` tag.**

#### Alpine Linux Container (Default)
![](https://img.shields.io/badge/image-quay.io%2Filoveyatoo%2Fpostal:alpine-blue?style=flat-square&logo=Docker) ![](https://img.shields.io/badge/image-docker.io%2Filoveyatoo%2Fpostal:alpine-blue?style=flat-square&logo=Docker) ![](https://img.shields.io/docker/image-size/iloveyatoo/postal/alpine?style=flat-square)

For this container, use the `alpine` folder.

#### Debian Linux Container
![](https://img.shields.io/badge/image-quay.io%2Filoveyatoo%2Fpostal:debian-blue?style=flat-square&logo=Docker) ![](https://img.shields.io/badge/image-docker.io%2Filoveyatoo%2Fpostal:debian-blue?style=flat-square&logo=Docker) ![](https://img.shields.io/docker/image-size/iloveyatoo/postal/debian?style=flat-square)

For this container, use the `debian` folder.

**Previously, this was incorrectly named as the "ubuntu" container, if you were using the ubuntu container, you can use this one instead - they are compatiable and the same**

### Instructions
1.  Change the folder to either `debian` or `alpine`, depending on which version you want to use
2.  Open `docker-compose.yml`
3.  Update `MYSQL_ROOT_PASSWORD` and `RABBITMQ_DEFAULT_PASS` everywhere in the file to new secret passwords
4.  Update `POSTAL_FNAME` (First Name), `POSTAL_LNAME` (Last Name), `POSTAL_PASSWORD`, and `POSTAL_EMAIL` values in the file
5.  Run `docker-compose up -d`

### Notes
*  The `POSTAL_EMAIL` and `POSTAL_PASSWORD` values will be the email and password you use to login
*  `POSTAL_EMAIL` and `POSTAL_PASSWORD` will only be used if there is a need to initialize the postal installation. As a result, changing them will not change the admin username or password while postal is already setup
*  All the setup instructions are already run

### Using the `postal` tool.
To use the `postal` tool, simply run
```
docker-compose run postal <parameter>
```
For example, the following command runs `postal initialize` inside the container.
```
docker-compose run postal initialize
```

### Migrations
See https://github.com/atech/postal/wiki/Upgrading. Note that building a new container (or pulling a new version from Docker Hub) will update the files in postal, so all you have to run is `postal upgrade` after building or retrieving the new container. Updating postal using its auto-update feature is highly not reccomended and likely does not work properly.

### Ports
Port mappings may change (as they have in the past). If SMTP/HTTP(s) is not working, verify that the mapped ports are correct.

### Anti-Spam / Antivirus
The initial design for the container was to be simple, minimal, and customizable, so Spamassassin and ClamAV are not included by default. Feel free to fork and add to the Dockerfile (though you must set docker-compose.yml to build from Dockerfile and not pull an image), or add them by linking additional containers.

### Managed Files
These files are automatically managed by the container. If you wish to adjust them, add them to the `persistent_assets` mount
- config/postal.yml
- config/fast_server.cert
- config/fast_server.key
- config/lets_encrypt.pem
- config/signing.key

### Updates
- v3.1.0
  * Fixed username/password injection problem during user creation by adding sleep in between entries
  * Updated mountpoint to use symbolic links instead of copying files to allow for app to update assets as needed
  * Fixed missing files that should be copied inbetween runs
- v3.0.0
  * Move to Gitlab with automated testing
  * Add user creation system for more reliability
  * Add some more checks on MySQL during startup

- v2.0.0
  * Update to more reliable version of YAML management system
  * Split into ubuntu and alpine images
  * Moved to CircleCI for more advanced building
- v1.0.0
  * Initial Release

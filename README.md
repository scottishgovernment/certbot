# Certbot Debian package for MyGov

This repository contains build scripts for a Debian package for Certbot.
The package installs Certbot in a Python virtualenv in /opt/certbot.
This helps to reduce the possibility of conflicts with other packages installed
on the system.

## Build

To ensure a clean build environment, the package is built with Docker.
On Ubuntu 16.04, first install docker and add the current user to the docker
group:

    apt-get install -y docker.io
    sudo adduser $USER docker
    exec sudo su - $USER

Create a build environment for the package by creating a container with
the required build dependencies:

    docker build -f Docker-build -t certbot-build .

This command should be repeated after changing the Build-Depends in the
debian/control file to keep the build environment up to date.

To build Certbot, run the container created in the previous step:

    docker run --rm -t -v $PWD:/mnt certbot-build

This leaves a certbot debian package and build log in the current
directory.

To test the package, install that package in a new container, and run
certbot:

    docker build -f Docker-run -t certbot .
    docker run -i -t certbot

## Upgrading

To upgrade to a later Certbot version, update the version number in debian/rules.

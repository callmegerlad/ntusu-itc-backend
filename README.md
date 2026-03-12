<div align="center">
  <img src="./docs/static/logo.webp" style="background-color:white;padding:1rem;border-radius:1rem;" alt="NTUSU ITC logo" width="75">
  <h1>NTUSU ITC Backend</h1>
  <p style="font-size:16px;font-weight:normal;">Backend repository for NTUSU ITC Portal</p>
</div>

<div align="center">
<p align="center">
<a href="#introduction">Introduction</a> &nbsp;&bull;&nbsp;
<a href="#system-overview">System Overview</a> &nbsp;&bull;&nbsp;
<a href="#getting-started">Getting Started</a> &nbsp;&bull;&nbsp;
<a href="#production-deployment">Production Deployment</a> &nbsp;&bull;&nbsp;
<a href="#feature-modules">Feature Modules</a>
</p>
</div>

## Introduction

NTUSU ITC Backend is the server-side application powering the NTUSU ITC Portal. Built with Django REST Framework, it provides APIs and core business logic for modules such as authentication, ULocker, events, and other student-facing services.

## System Overview

### System Architecture

![System Architecture Diagram for NTUSU-ITC Backend](./docs/static/NTUSU_ITC-System_Achitecture.png "System Architecture Diagram")


### Tech Stack

![Tech Stack (Cloud)](https://img.shields.io/badge/cloud%3A-222222?style=for-the-badge) &nbsp; ![AWS Badge](https://img.shields.io/badge/Amazon_Web_Services-232F3E?style=for-the-badge&logo=amazonwebservices&labelColor=202020)
  ![Google Cloud Badge](https://img.shields.io/badge/Google_Cloud-4285F4?style=for-the-badge&logo=googlecloud&labelColor=202020)

![Tech Stack (Containerisation)](https://img.shields.io/badge/containerisation%3A-222222?style=for-the-badge) &nbsp; ![Docker Badge](https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&labelColor=222222)

![Tech Stack (Backend)](https://img.shields.io/badge/backend%3A-222222?style=for-the-badge) &nbsp; ![Django Badge](https://img.shields.io/badge/Django_REST-092E20?style=for-the-badge&logo=django&labelColor=222222&color=092E20) ![Python Badge](https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&labelColor=222222)

![Tech Stack (Database)](https://img.shields.io/badge/database%3A-222222?style=for-the-badge) &nbsp; ![MySQL Badge](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&labelColor=222222)



## Getting Started

This section will guide you through setting up the project on your local machine to begin contributing to development!

### Pre-requisites

- Docker Desktop ([MAC](https://docs.docker.com/desktop/install/mac-install/) / [Windows](https://docs.docker.com/desktop/install/windows-install/) / [Linux](https://docs.docker.com/desktop/install/linux-install/))

### 1. Clone this repository

Clone this repository and move into the project directory where `docker-compose.yml` file is located

### 2. Build and run application using Docker

Build the images and start the containers with the command:

```powershell
docker-compose up --build
```

> 📝 Note: To stop the application, hit the `CTRL` + `C` keys simultaneously in the terminal!

Your application should now be up and running at [localhost:8888](http://localhost:8888/). 

### 3. Initialise database migration

Using another terminal, execute the following command:

```powershell
docker exec -ti SUITC_Backend python manage.py migrate
```

You should perform this command when it is your first time running the development environment, and anytime there are new database migrations.

### Troubleshoot

This section is to document any problems that might occur when running the development environment along with its solution, as we all are using different devices which might have slightly different behaviour.

- Note: For Macbook M1/M2, you must run this before doing all required steps below:

```powershell
softwareupdate --install-rosetta
export DOCKER_DEFAULT_PLATFORM=linux/amd64
```

### Executing Utility Commands

In order for you to execute other `manage.py` utility commands through Docker container, just add `docker exec -ti SUITC_Backend` (add `sudo` if you're using MAC) in front of your command. Please ensure that the server is indeed running, open another terminal if needed.

For example, to create new migrations you can run:

```powershell
docker exec -ti SUITC_Backend python manage.py makemigrations
```

### Testing

Everytime you add new features, please create the appropriate test cases. You can run this command to run all the test cases:

```powershell
docker exec -ti SUITC_Backend python manage.py test
```

Automatic CI testing is enforced everytime a pull request or a push is done to the main branch.

### Superuser & Sample Data

Sample data are generated using Django Fixtures. It is used to populate your database (stored in Docker Volumes) with some sample data so that every person do not have to do it on their own manually. You can load the data by opening another terminal and run the command:

```powershell
docker exec -ti SUITC_Backend python manage.py loaddata sample_user
```

Running this command will give you superuser access with the username `superuser` and password `123`. Other sample data can be seen on the fixtures folder (password for other sample user: 1048576#).

Warning: running the command may overwrite some existing data

### API Documentation

There are 2 types of documentation provided here:

- Automatic Documentation using Swagger UI

- Manual Documentation in the `docs` app by writing markdown files (stored locally in this repository) (NOTE: this is not available in the live environment yet)

See some of the manual documentations here:

- [Index Swapper](/docs/api-guide/index%20swapper.md)

## Production Deployment

The live server is deployed using AWS Elastic Beanstalk (environment name: `Ntusuitcbackendprod-env`) [here](https://backend.ntusu.org/) using Python 3.8 running on Linux2 Ver 3.4.3 machine. Current database is stored using EC2 instance using MySQL (RDS is so expensive so we're not using this), static files are stored in S3 buckets. Database and S3 storage related configurations are set up through environment variables in the EB environment. SSL certificate issued through AWS certificate manager, dns using Route 53.

In order to manually type Django manage.py commands, you have to first download [AWS EB CLI](https://github.com/aws/aws-elastic-beanstalk-cli-setup), then connect via ssh by typing `eb ssh Ntusuitcbackendprod-env`, enter secret key pair, and finally type these commands referenced [here](https://stackoverflow.com/a/71045510).

```powershell
eb ssh
sudo su -
export $(cat /opt/elasticbeanstalk/deployment/env | xargs)
source /var/app/venv/*/bin/activate
python3 /var/app/current/manage.py <command name>
```

Important note: every time there are new migrations, please connect via ssh to perform migration in the live server (later please try to automate this process!)

## Feature Modules

### Portal

### SSO

### UFacility

### ~~Inventory~~

### Event

### Docs

### StarsWar

### ~~ULocker~~

### Workshop

### Communication

### Games

### UShop

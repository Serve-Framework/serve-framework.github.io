# Installation

--------------------------------------------------------
* [Requirements](#requirements)
* [Installation](#installation)
	* [With Composer](#install-with-composer)
	* [Deployment Ready](#deployment-ready)
--------------------------------------------------------

### Requirements

* GIT
* PHP >= 8.0.8
* PHP Extensions:
	* OpenSSL PHP Extension
	* PDO PHP Extension
* Composer
* Web Server With SQL

--------------------------------------------------------

### Installation
The best way to get started with Serve is [With Composer](#install-with-composer). 

--------------------------------------------------------
### Install With Composer

Installing Serve is easy and can be with done in a few simple steps thanks to composer.

1. First you'll have to create a new project:

```bash
# cd /home/public_html
composer create-project serve/app .
```

2. Next you'll have to make the `app/storage` and  `app/public/uploads` directories writable (command my vary depending on your system):

```bash
chown www-data:www-data -R app/storage
chown www-data:www-data -R app/public/uploads
```

3. You'll also need to create a database for Serve on your web server, as well as a MySQL user who has all privileges for accessing and modifying it. You can create a database using phpMyAdmin or whatever means you prefer.

4. Add your database credentials into the `app/configurations/database.php` file. The default credentials are setup to run on most localhost default web servers.

5. If you need to use the `Gatekeeper` service, you'll need to create the necessary users table. See the [Gatekeeper Documentation](/8.0.0/09_security/02_gatekeeper?id=schema) for further details.

6. You'll have to make your `app/storage` and `app/public/uploads` directories writable (command my vary depending on your system):

```bash
chown www-data:www-data -R app/storage
chown www-data:www-data -R app/public/uploads
```

7. Add a `.gitignore` file to your `app/configurations` directory to ensure any custom configuration settings you make are not overwritten.

```
*
!.gitignore
```

> Please see the [Configuration](/getting-started/configuratio) section on configuration your Serve installation.

#### Updating

Serve can easily be updated when a new version is released using the following command:

```bash
composer update
```

--------------------------------------------------------

### Deployment Ready
For production-ready environments, you may want to install Serve using your own Github repository that you can push/pull changes to.

The advantages with this installation method is that you will have your own repository on Github for your project which you can deploy directly to your server using [Webhooks](https://developer.github.com/webhooks/).

1. On Github, [create a fork](https://help.github.com/articles/fork-a-repo/) of the `Serve-Framework/Framework` repository.

2. Clone your Serve fork to your local machine using [Github Desktop](https://desktop.github.com/).

3. Using terminal, navigate to your fork's directory.

4. Add a second remote to the original Serve repository.

```bash
git remote add upstream https://github.com/Serve-Framework/Framework.git
```

#### Updating

Serve can easily be updated when a new version is released using the following command:

```bash
git fetch upstream
git checkout master
git merge upstream/master
```

> For further details on deploying your project see the [Deployment](/deployment) section of the documentation.

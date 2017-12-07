# Installation Instructions

- [0. Introduction](#0-introduction)
- [1. Install Composer](#1-install-composer)
- [2. Create a New Craft Project](#2-create-a-new-craft-project)
- [3. Set up the Database](#3-set-up-the-database)
- [4. Set up the Web Server](#4-set-up-the-web-server)

## 0. Introduction

Craft 3 is available as a [Composer] package, and for the duration of the Beta, Composer is the only way to install Craft 3. (We’ll introduce an alternate, non-Composer installation method once it’s out of Beta.) 

If you’re unfamiliar with Composer it’s a package manager for PHP that attempts to make installing and updating PHP libraries (like Craft) easy via the command line.

Craft’s Composer support has two parts:

1. **[`craftcms/cms`]** – This is the Composer package that contains all of Craft’s source code.
2. **[`craftcms/craft`]** – This is a Composer “project” that can be used as a starting point for new Craft projects.

## 1. Install Composer

You should be running Composer 1.3.0 or later. You can find out your installed version of Composer (if any) by opening your terminal running the following command:

    composer -V

If that outputs an error saying `composer` is not found or not recognized, then Composer isn’t installed. Follow Composer’s instructions to install it:

  - [macOS/Linux/Unix instructions] *(install it globally)*
  - [Windows instructions]

If it outputs a version number, but it’s less than `1.3.0`, run the following command to update it:

    composer self-update

## 2. Create a New Craft Project

To create a new Craft project, run this command (substituting `PATH` with the path where Composer should create the project):

    composer create-project craftcms/craft PATH -s beta

Note: If Composer complains that your system doesn’t have PHP 7 installed, but you know it’s not an issue because Craft will run with a different PHP install (e.g. through MAMP or Vagrant), use the `--ignore-platform-reqs` flag.

Composer will take a few minutes to install everything.

Once it’s finished, your project directory should have a file structure like this:

```
config/...
storage/
templates/
vendor/...
web/...
.env
.env.example
composer.json
craft
craft.bat
LICENSE.md
README.md
```

See [Directory Structure](directory-structure.md) for information on these directories and files.

## 3. Setting Up the Database

Next up, you need to create a database for your Craft project. Craft 3 supports both MySQL 5.5+ and PostgreSQL 9.5+.

If you’re given a choice, we recommend the following database settings in most cases:

- **MySQL**
  - Default Character Set: `utf8`
  - Default Collation: `utf8_unicode_ci`

- **PostgreSQL**
  - Character Set: `UTF8`

Once you create the database, you’ll need to configure your `.env` file with  the database connection settings. You can either edit the file manually, or run the `./craft setup` command from the root project directory in your terminal.

Note:  [PHP dotenv](https://github.com/vlucas/phpdotenv), which the `craftcms/craft` project comes with pre-installed, processes the `.env` file. The advantage of using PHP dotenv is that it offers a place to store sensitive information (like database connection settings) in a file that doesn’t get committed to your Git repository.

## 4. Set up the Web Server

Create a new web server to host your Craft project. Its document root should point to your public HTML folder. If you’re using Craft’s Composer [starter project](https://github.com/craftcms/craft), then it is the `web/` directory by default, but it can be renamed to anything you want as long as your web server is configured to point to it.

If your web host already has a public HTML folder setup to for you that you cannot rename, then you can just copy the contents of Craft’s `web` folder to it and use your host’s default public HTML folder. 

If you’re not using [MAMP](https://mamp.info) or another localhosting tool, you will probably need to update your `hosts` file, so your computer knows to route requests to your chosen host name to the local computer.

- **macOS/Linux/Unix:** `/etc/hosts`
- **Windows:** `\Windows\System32\drivers\etc\hosts`

You can test whether you set everything up correctly by pointing your web browser to `http://HOSTNAME/index.php?p=admin` (substituting `HOSTNAME` with your new web server’s host name). 

If successful, you will get the Craft installation wizard. The wizard will take you through a couple setup screens, and then perform the installation of Craft.


[Composer]: https://getcomposer.org/
[`craftcms/cms`]: https://github.com/craftcms/cms
[`craftcms/craft`]: https://github.com/craftcms/craft
[Composer installer]: https://getcomposer.org/doc/articles/custom-installers.md
[project]: https://github.com/craftcms/craft
[macOS/Linux/Unix instructions]: https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx
[Windows instructions]: https://getcomposer.org/doc/00-intro.md#installation-windows
[PHP dotenv]: https://github.com/vlucas/phpdotenv

Installation Instructions
=========================

- [Introduction](#0-introduction)
- [Installing Craft with Composer]()
- [Installing Craft Manually]()
- [Setting up the Database](#3-set-up-the-database)
- [Setting up the Web Server](#4-set-up-the-web-server)

## Introduction

Craft 3 is available as a [Composer] package and as a stand-alone download. We encourage you to use the Composer package for easier installation. If you’re unfamiliar with Composer, it’s a package manager for PHP, meaning it’s a tool that attempts to make installing and updating PHP libraries (like Craft) a simple terminal command away.

Before you get started, be sure to review the [requirements for running Craft](requirements.md).

Craft’s Composer support is made up of three parts:

1. **[`craftcms/cms`]** – Composer *package* that contains all of Craft’s source code and bootstrap scripts.
2. **[`craftcms/plugin-installer`]** – Custom Composer *installer* that makes it possible to install Craft plugins with Composer.
2. **[`craftcms/craft`]** – Composer *project* that can be installed as a starting point for new Craft projects, with the `cms` and `plugin-installer` dependencies already in place.

## Installing Craft with Composer

### 1. Install Composer

You can find out if Composer is already installed by opening your terminal and entering one of the following commands:

- **macOS/Linux/Unix**

      which composer

- **Windows**

      where composer

If that outputs a file path(s), Composer is installed. Otherwise you will need to follow Composer’s installation instructions:

  - [macOS/Linux/Unix instructions] *(install it globally)*
  - [Windows instructions]

### 2. Create a New Craft Project

To create a new Craft project, simply run this command (substituting `PATH` with the path the project should be created at):

    composer create-project craftcms/craft PATH -s beta

> {tip} If Composer complains that your system doesn’t have PHP 7 installed, but you know it’s not an issue because Craft will run with a different PHP install (e.g. through MAMP or Vagrant), use the `--ignore-platform-reqs` flag.

Composer will take a few minutes to install everything, so this would be a great time to make yourself some coffee or a cocktail.

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

See [Directory Structure](directory-structure.md) for a rundown of what each of those directories and files are for.

Move on to Step 3 to complete your Craft installation.

## Installing Craft Manually

### Pre-flight check

Before installing Craft, make sure that you’ve got everything you need:

* The latest version of Craft from [craftcms.com](https://craftcms.com).
* A web host meets Craft’s [minimum requirements]({entry:docs/requirements:url}).
* An FTP client (we recommend [Transmit](http://panic.com/transmit/)).
* MySQL access, either via a web-based tool like phpMyAdmin, or a standalone app like [Sequel Pro](http://www.sequelpro.com/).
* Your favorite text editor
* Your good looks

### Step 1: Upload the files

Extract your Craft zip somewhere on your computer. You’ll notice that it contains two folders:

* craft/
* public/

The **craft/** folder contains [all kinds of stuff]({entry:docs/folder-structure} "{entry:docs/folder-structure:title}"), from the actual application files to configuration files to your templates. This folder should be uploaded in its entirety to your server.

We recommend that you upload the folder *above* your web root if possible, which will ensure that no one can access any of its files directly. (Your web root is the folder that your domain name points to.) That’s not a requirement, but do it if you can. For the children.

The **public/** folder contains a few files that can go inside your web root. The only file that’s actually required here is **index.php**, which is the web’s official entry point into your Craft site.

By default the index.php file assumes that you uploaded the craft/ folder one level above it. For example, on your server it might look like this:

    craft/
    public_html/
        index.php

If that’s not the case, you will need to open up your index.php file and change the `$craftPath` variable to point to the actual location of your craft/ folder. If the craft/ folder lives right next to your index.php file, you would change that line to this:

    $craftPath = './craft';

The other files in public/ are all optional. Here’s what they do:

* **htaccess** – This file configures Apache servers to direct all traffic hitting your site to that index.php file, without actually needing to include “index.php” in the URLs. Note that it must be renamed to **.**htaccess for it to actually work. (See “{entry:supportArticles/remove-index.php:link}” for more info.)
* **web.config** – This is our IIS equivelant of the .htaccess file, for those of you that are into that sort of thing.
* **robots.txt** – If you couldn’t upload the craft/ folder above your web root, you can use this file to prevent Google from indexing it.

### Step 2: Set the permissions

At a minimum, Craft needs to be able to write to 3 folders on your server:

* craft/app/
* craft/config/
* craft/storage/

Additionally, if you define any [Local Asset sources](https://craftcms.com/docs/assets) in your public HTML folder, Craft will need to be able to write to them as well.

In order for Craft to be able to do that, it’s counting on you to set the needed permissions on those folders (and each of their subfolders).

The exact permissions you should be setting depends on the relationship between the system user that PHP is running as, and who owns the actual folders/files.

Here are some recommended permissions depending on that relationship:

* If they are the same user, use 744.
* If they're in the same group, then use 774.
* If they’re neither the same user nor in the same group, or if you just prefer to live life on the edge, you can use 777, just please do not do that in a production environment.

**IIS fans:** Make sure the account your site’s AppPool is running as has write permissions to this folder.


## Set up the Database

Next up, you’ll need to create a database for your Craft project. Craft 3 supports both MySQL 5.5+ and PostgreSQL 9.5+.

If you’re given a choice, we recommend the following database settings in most cases:

- **MySQL**
  - Default Character Set: `utf8`
  - Default Collation: `utf8_unicode_ci`

- **PostgreSQL**
  - Character Set: `UTF8`

Once the database is created, you’ll need to configure Craft with its connection settings. Open the `.env` file at the root of your Craft project and fill in your database connection settings.

> {tip} That `.env` file will be processed via [PHP dotenv], which the `craftcms/craft` project comes with preinstalled. The advantage of using PHP dotenv is that it offers a place to store sensitive information (like database connection settings) in a file that doesn’t get committed to your Git repository.

## Set up the Web Server

Create a new web server to host your Craft project. Its document root should point to the `web/` folder.

If you’re not using MAMP, you will probably need to update your `hosts` file, so your computer knows to route requests to your chosen host name to the local computer.

- **macOS/Linux/Unix:** `/etc/hosts`
- **Windows:** `\Windows\System32\drivers\etc\hosts`

## Run the installer!

Now that everything’s set up, point your browser to <http://example.com/admin>. You should get the Craft installation wizard, which will take you through a couple setup screens, and then perform the actual installation.


[Composer]: https://getcomposer.org/
[`craftcms/cms`]: https://github.com/craftcms/cms
[`craftcms/plugin-installer`]: https://github.com/craftcms/plugin-installer
[`craftcms/craft`]: https://github.com/craftcms/craft
[Composer installer]: https://getcomposer.org/doc/articles/custom-installers.md
[project]: https://github.com/craftcms/craft
[macOS/Linux/Unix instructions]: https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx
[Windows instructions]: https://getcomposer.org/doc/00-intro.md#installation-windows
[PHP dotenv]: https://github.com/vlucas/phpdotenv

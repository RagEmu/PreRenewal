RagEmu
========

Build Status:  
[![Build Status](https://travis-ci.org/RagEmu/PreRenewal.svg)](https://travis-ci.org/RagEmu/PreRenewal)
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/bkmmcsn3pcx8lrpa?svg=true)](https://ci.appveyor.com/project/dastgir/prerenewal)

Project Info:  
![Language](https://img.shields.io/badge/language-C-yellow.svg)
[![License](https://img.shields.io/badge/license-GPLv3-663399.svg)](https://github.com/RagEmu/PreRenewal/blob/master/LICENSE)

Table of Contents
---------
* 1 What is RagEmu?
* 2 Prerequisites
* 3 Installation
* 4 Troubleshooting
* 5 Helpful Links
* 6 More Documentation

What is RagEmu?
-----------------
RagEmu is a collaborative software development project revolving around the
creation of a robust massively multiplayer online role playing game (MMORPG)
server package. Written in C, the program is very versatile and provides NPCs,
warps and modifications. The project is jointly managed by a group of
volunteers located around the world as well as a tremendous community providing
QA and support. RagEmu is a continuation of the original Athena project.

Prerequisites
-------------
Before installing RagEmu there are certain tools and applications you will
need.  This differs between the varying operating systems available, so the
following is broken down into Windows and Unix (incl. Linux) prerequisites.

For a list of supported platforms, please refer to the [Supported
Platforms](https://github.com/HerculesWS/Hercules/wiki/Supported-Platforms) wiki page.

#### Windows
  - Git client
  - MySQL-compatible server ([MySQL Community Edition](https://www.mysql.com/products/community/) or
    [MariaDB](https://mariadb.org/))
  - Microsoft Visual Studio ([Version 2012 through 2015](https://www.visualstudio.com/))

#### Unix/Linux/BSD (names of packages may require specific version numbers on certain distributions)
  - git
  - gcc or clang (version 4.5 or newer, recommended 5.0 or newer)
  - GNU make
  - mysql (`mysql-server`) or mariadb
  - libmysqlclient (`mysql-devel`)
  - zlib (`zlib-devel`)
  - libpcre (`pcre-devel`)
  - *Optional dependencies for development only*
    - perl (required to rebuild the HPM Hooks and HPMDataCheck)
    - Doxygen (required to rebuild the HPM Hooks and HPMDataCheck)

#### Mac OS X
  - Xcode or the Xcode command-line tools.
  - MySQL-compatible server (installation of `mysql` or `mariadb` through [Homebrew](http://brew.sh/) is recommended)
  - PCRE library (installation of `pcre` through [Homebrew](http://brew.sh) is recommended)
  - *Optional dependencies for development only*
    - Doxygen (required to rebuild the HPM Hooks and HPMDataCheck)

#### Optional, useful tools
  - MySQL GUI clients
    - [MySQL Workbench](http://www.mysql.com/downloads/workbench/) (cross-platform)
    - [HeidiSQL](http://www.heidisql.com/) (Windows)
    - [Sequel Pro](http://www.sequelpro.com/) (Mac OS X)
  - GUI Git clients
    - [Atlassian SourceTree](https://www.sourcetreeapp.com/) (Windows, Mac OS X)
    - [TortoiseGit](https://tortoisegit.org/) (Windows)


Installation
------------

This section is a very brief set of installation instructions. For more concise guides
relevant to your Operation System, please refer to the Wiki (links at the end of this file).

#### Windows
  1. Install the prerequisites.
  2. Clone the RagEmu repository (see [GitHub](https://github.com/RagEmu/PreRenewal)) using a git client, into a new folder.
  3. Connect to the MySQL server as root:
    - Create a database (RagEmu): `CREATE DATABASE RagEmu;`
    - Create a user (RagEmu): `CREATE USER 'RagEmu'@'localhost' IDENTIFIED BY 'password';`.
    - Give permissions (GRANT SELECT,INSERT,UPDATE,DELETE) to the user: `GRANT SELECT,INSERT,UPDATE,DELETE ON \`RagEmu\`.* TO 'RagEmu'@'localhost';`
  4. Connect to the MySQL server as the new user:
    - Import the .sql files in /sql-files/ into the new database.
  5. Start Visual Studio and load the provided solution:
    - Compile and run the three projects, login-server, char-server, map-server.

#### Unix
  1. Install the prerequisites through your distribution's package manager
    - (Red Hat compatible / CentOS) `yum install gcc make mysql mysql-devel mysql-server pcre-devel zlib-devel git`
    - (Debian compatible) `apt-get install gcc make libmysqlclient-dev zlib1g-dev libpcre3-dev mysql-server git`
    - (FreeBSD) `pkg install clang35 gmake mysql56-server mysql-connector-c pcre git`
    - (Mac OS X):
      - Install Xcode through the Mac App Store
      - Initialize the build tools through the Terminal `xcode-select --help`
      - Install Homebrew as described on the project page
      - Install MySQL: `brew install mysql`
  2. Clone the RagEmu repository `git clone https://github.com/RagEmu/PreRenewal.git ~/RagEmu`
  3. Configure the MySQL server and start it.
  4. Connect to the MySQL server as root:
    - Create a database (RagEmu): `CREATE DATABASE RagEmu;`
    - Create a user (RagEmu): `CREATE USER 'RagEmu'@'localhost' IDENTIFIED BY 'password';`.
    - Give permissions (GRANT SELECT,INSERT,UPDATE,DELETE) to the user: `GRANT SELECT,INSERT,UPDATE,DELETE ON \`RagEmu\`.* TO 'RagEmu'@'localhost';`
  5. Connect to the MySQL server as the new user:
    - Import the .sql files in /sql-files/ into the new database.
  6. Enter the RagEmu directory and configure/build RagEmu
    - `./configure`
    - `make clean && make sql` (on FreeBSD, replace `make` with `gmake`)
  7. Start the three servers login-server, char-server, map-server.

Troubleshooting
---------------

If you're having problems with starting your server, the first thing you should
do is check what's happening on your consoles. More often that not, all support
issues can be solved simply by looking at the error messages given.

Examples:

* You get an error on your map-server_sql that looks something like this:

		[Error]: npc_parsesrcfile: Unable to parse, probably a missing or extra TAB in 
			file 'npc/custom/jobmaster.txt', line '17'. Skipping line...
			* w1=prontera,153,193,6 script
			* w2=Job Master
			* w3=2_F_MAGICMASTER,{
			* w4=

	If you look at the error, it's telling you that you're missing (or have an extra) TAB.
		This is easily fixed by looking at this part of the error: * w1=prontera,153,193,6 script
		If there was a TAB where it's supposed to be, that line would have prontera,153,193,6 at w1
		and 'script' at w2. As there's a space instead of a TAB, the two sections are read as a
		single parameter.

* You have a default user/password warning similar to the following:
		
			[Warning]: Using the default user/password s1/p1 is NOT RECOMMENDED.
			[Notice]: Please edit your 'login' table to create a proper inter-server user/pa
			ssword (gender 'S')
			[Notice]: and then edit your user/password in conf/map-server.conf (or conf/impo
			rt/map_conf.txt)

	Relax. This is just indicating that you're using the default username and password. To
		fix this, check over the part in the installation instructions relevant to the `login` table.

* Your Map Server says the following:

			[Error]: make_connection: connect failed (socket #2, error 10061: No connection
			could be made because the target machine actively refused it.
			)!

	If this shows up on the map server, it generally means that there is no Char Server available
		to accept the connection.

Helpful Links
-------------

The following list of links point to various help files within the GIT, articles or
pages on the Wiki or topics within the RagEmu forum.

* RagEmu Forums:
	http://ragemu.org/board/


* GIT Repository URL:
	https://github.com/RagEmu/PreRenewal


More Documentation
------------------

RagEmu has a large collection of help files and sample NPC scripts located in /doc/

* Scripting
	It is recommended to look through /doc/script_commands.txt for help, pointers or
	even for ideas for your next NPC script. Most script commands have a usage example.

* @commands
	In-game, Game Masters have the ability to use Atcommands (@) to control players, 
	create items, spawn mobs, reload configuration files and even control the weather.
	For an in-depth explanation, please see /doc/atcommands.txt

* Permissions
	The RagEmu emulator has a permission system that enables certain groups of players
	to perform certain actions, or have access to certain visual enhancements or in-game
	activity. To see what permissions are available, they are detailed in /doc/permissions.txt

There are more files in the /doc/ directory that will help you to create scripts or update the
mapcache, or even explain how the job system and item bonuses work. Before posting a topic asking
for help on the forums, we recommend that all users take the time to look over this directory.

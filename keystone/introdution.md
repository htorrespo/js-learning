# KeystoneJS (Node.js CMS & Web Application Platform)

## Introduction of KeystoneJS

KeystoneJS is a generic content management framework, meaning that it 
can be used for developing a variety of web applications using Javascript. 
It is especially suitable for developing large-scale applications such 
as portals, forums, content management systems (CMS), e-commerce projects 
and RESTful Web services because of its modular architecture and clean 
separation of various functionality. KeystoneJS is a powerful Node.js 
content management system and web app framework built on express (https://expressjs.com/) and 
mongoose (https://mongoosejs.com/). Keystone makes it easy to create sophisticated websites and 
apps and comes with a beautiful auto-generated Admin UI.

## Install dependencies

You’ll be using the KeystoneJS generator (https://github.com/keystonejs/generator-keystone) made with Yeoman
(https://yeoman.io/). 

In your root directory run:

```terminal
npm install -g generator-keystone
```

One-line install Yeoman using npm:

```terminal
npm install -g yo
```

Create your project wherever you want:

```terminal
mkdir keystone
```

Then make sure you’re in your new project:

```terminal
cd keystone
```

Run the new project generator

```terminal
yo keystone
```

The generator will ask you a few questions about what features you’d 
like to include, then configure and copy all the files you’ll need 
into your project.

It will also install dependencies from npm so you’re ready to go.

Run it in your command line like this:

```terminal
node keystone
```

The installation is ...

```terminal
Welcome to KeystoneJS.

? What is the name of your project? home-ximena
? Would you like to use Pug, Nunjucks, Twig or Handlebars for templates? [pug | nunjucks | twig | hbs] pug
? Which CSS pre-processor would you like? [less | sass | stylus] saas
? Would you like to include a Blog? Yes
? Would you like to include an Image Gallery? Yes
? Would you like to include a Contact Form? Yes
? What would you like to call the User model? User
? Enter an email address for the first Admin user: henrytorrespo@yahoo.com
? Enter a password for the first Admin user:
 Please use a temporary password as it will be saved in plain text and change it after the first login. 12345
? Would you like to create a new directory for your project? Yes
? ------------------------------------------------
    Would you like to include Email configuration in your project?
    We will set you up with an email template for enquiries as well
    as optional mailgun integration Yes
? ------------------------------------------------
    If you want to set up mailgun now, you can provide
    your mailgun credentials, otherwise, you will
    want to add these to your .env later.
    mailgun API key:
? ------------------------------------------------
    mailgun domain:
? ------------------------------------------------
    KeystoneJS integrates with Cloudinary for image upload, resizing and
    hosting. See http://keystonejs.com/docs/configuration/#services-cloudinary for more info.

    CloudinaryImage fields are used by the blog and gallery templates.

    You can skip this for now (we'll include demo account details)

    Please enter your Cloudinary URL:
? ------------------------------------------------
    Finally, would you like to include extra code comments in
    your project? If you're new to Keystone, these may be helpful. Yes
   create package.json
   
   added 1137 packages, and audited 1138 packages in 44s

13 packages are looking for funding
  run `npm fund` for details

76 vulnerabilities (29 low, 10 moderate, 37 high)

To address issues that do not require attention, run:
  npm audit fix

To address all issues, run:
  npm audit fix --force

Run `npm audit` for details.

------------------------------------------------

Your KeystoneJS project is ready to go!

For help getting started, visit http://keystonejs.com/guide

We've included the setup for email in your project. When you want to get 
this working, just create a mailgun account and putyour mailgun details 
into the .env file.

We've included a demo Cloudinary Account, which is reset daily.
Please configure your own account or use the LocalImage field instead
before sending your site live.

To start your new website, run "cd home-ximena" then "node keystone".

┌───────────────────────────────────────────────────────────┐
│                  yo update check failed                   │
│            Try running with sudo or get access            │
│           to the local update config store via            │
│ sudo chown -R $USER:$(id -gn $USER) /home/itsuito/.config │
└───────────────────────────────────────────────────────────┘

```

Run it in your command line like this:

```terminal
node keystone
```

Then open http://localhost:3000 to view it in your browser.


Install MongoDB from Default Ubuntu Repositories (Easy)
https://phoenixnap.com/kb/how-to-install-mongodb-ubuntu


How to fix 'System has not been booted with systemd' error?
https://linuxhandbook.com/system-has-not-been-booted-with-systemd/

```terminal
itsuito@DESKTOP-RJ2U4HK:~/keystone/home-ximena$ sudo service mongodb
Usage: /etc/init.d/mongodb {start|stop|force-stop|restart|force-reload|status}
```
```terminal
itsuito@DESKTOP-RJ2U4HK:~/keystone/home-ximena$ service mongodb start
 * Starting database mongodb                                                                                            
 * install: cannot change owner and permissions of ‘/run/mongodb’: No such file or directory
start-stop-daemon: unable to open pidfile '/run/mongodb/mongodb.pid' for writing (No such file or directory)
start-stop-daemon: unable to set gid to 120 (Operation not permitted)
start-stop-daemon: child returned error exit status 2 (No such file or directory)

itsuito@DESKTOP-RJ2U4HK:~/keystone/home-ximena$ sudo service mongodb start
 * Starting database mongodb

itsuito@DESKTOP-RJ2U4HK:~/keystone/home-ximena$ service --status-all
 [ - ]  acpid
 [ + ]  apache-htcacheclean
 [ - ]  apache2
 [ - ]  apparmor
 [ ? ]  apport
 [ - ]  atd
 [ - ]  avahi-daemon
 [ + ]  cgroupfs-mount
 [ - ]  console-setup.sh
 [ - ]  cron
 [ ? ]  cryptdisks
 [ ? ]  cryptdisks-early
 [ - ]  dbus
 [ - ]  docker
 [ - ]  ebtables
 [ ? ]  hwclock.sh
 [ + ]  irqbalance
 [ + ]  iscsid
 [ - ]  keyboard-setup.sh
 [ ? ]  kmod
 [ - ]  lvm2
 [ + ]  lvm2-lvmetad
 [ + ]  lvm2-lvmpolld
 [ - ]  lxcfs
 [ - ]  lxd
 [ - ]  mdadm
 [ - ]  mdadm-waitidle
 [ + ]  mongodb
 [ + ]  open-iscsi
 [ - ]  open-vm-tools
 [ ? ]  plymouth
 [ ? ]  plymouth-log
 [ - ]  postgresql
 [ - ]  procps
 [ - ]  rsync
 [ - ]  rsyslog
 [ - ]  screen-cleanup
 [ - ]  ssh
 [ - ]  sysstat
 [ ? ]  ubuntu-fan
 [ - ]  udev
 [ - ]  ufw
 [ - ]  unattended-upgrades
 [ - ]  uuidd
 [ - ]  x11-common
 
 itsuito@DESKTOP-RJ2U4HK:~/keystone/home-ximena$ node keystone
----------------------------------------
WARNING: MISSING MAILGUN CREDENTIALS
----------------------------------------
You have opted into email sending but have not provided
mailgun credentials. Attempts to send will fail.

Create a mailgun account and add the credentials to the .env file to
set up your mailgun integration
------------------------------------------------
Applying update 0.0.1-admins...

------------------------------------------------
home-ximena: Successfully applied update 0.0.1-admins.

Successfully created:

*   1 User


------------------------------------------------
Successfully applied 1 update.
------------------------------------------------

------------------------------------------------
KeystoneJS Started:
home-ximena is ready on http://0.0.0.0:3000
------------------------------------------------

```

https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database

Install MongoDB

To install MongoDB on WSL (Ubuntu 18.04):

1. Open your WSL terminal (ie. Ubuntu 18.04).
2. Update your Ubuntu packages: _sudo apt update_
3. Once the packages have updated, install MongoDB with: _sudo apt-get install mongodb_
4. Confirm installation and get the version number: _mongod --version_

There are 3 commands you need to know once MongoDB is installed:

    sudo service mongodb status for checking the status of your database.
    sudo service mongodb start to start running your database.
    sudo service mongodb stop to stop running your database.

Note

You might see the command sudo systemctl status mongodb used in tutorials or articles. In order to remain lightweight, WSL does not include systemd (a service management system in Linux). Instead, it uses SysVinit to start services on your machine. You shouldn't notice a difference, but if a tutorial recommends using sudo systemctl, instead use: sudo /etc/init.d/. For example, sudo systemctl status mongodb, for WSL would be sudo /etc/inid.d/mongodb status ...or you can also use sudo service mongodb status.

To run your Mongo database in a local server:

1. Check the status of your database: sudo service mongodb status You should see a [Fail] response, unless you've already started your database.

2. Start your database: sudo service mongodb start You should now see an [OK] response.

3. Verify by connecting to the database server and running a diagnostic command: mongo --eval 'db.runCommand({ connectionStatus: 1 })' This will output the current database version, the server address and port, and the output of the status command. A value of 1 for the "ok" field in the response indicates that the server is working.

4. To stop your MongoDB service from running, enter: sudo service mongodb stop

Note

MongoDB has several default parameters, including storing data in /data/db and running on port 27017. Also, mongod is the daemon (host process for the database) and mongo is the command-line shell that connects to a specific instance of mongod.

VS Code supports working with MongoDB databases via the Azure CosmosDB extension, you can create, manage and query MongoDB databases from within VS Code. To learn more, visit the VS Code docs: Working with MongoDB (https://code.visualstudio.com/docs/azure/mongodb).

Learn more in the MongoDB docs:

- Introduction to using MongoDB
- Create users
- Connect to a MongoDB instance on a remote host
- CRUD: Create, Read, Update, Delete
- Reference Docs


Revisar el siguente link
https://v4.keystonejs.com/getting-started/yo-generator


&_____________________________________________________________________

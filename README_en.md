[中文版](README.md)

# odoo-docker-tutorial

This branch uses Odoo 12 as an example. For an Odoo 15 example, please refer to the [15.0 branch](https://github.com/twtrubiks/odoo-docker-tutorial/tree/15.0).

## Table of Contents

1. [Introduction to Odoo](https://github.com/twtrubiks/odoo-docker-tutorial#introduction) - [Youtube Tutorial - Quickly set up an Odoo environment with Docker - from scratch](https://youtu.be/uqxzq4Td6aU)

2. [Quickly set up an Odoo environment with Docker](https://github.com/twtrubiks/odoo-docker-tutorial#tutorial) - [Youtube Tutorial - Quickly set up an Odoo environment with Docker - from scratch](https://youtu.be/uqxzq4Td6aU)

3. [How to enable Odoo developer mode in Odoo 12](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-enable-odoo-developer-mode-in-odoo-12) - [Youtube Tutorial - How to enable Odoo developer mode in Odoo 12](https://youtu.be/fUtqWQHbt1I)

4. [How to install third-party addons](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-install-third-party-addons) - [Youtube Tutorial - How to install addons in Odoo](https://youtu.be/aN35xlUBKwc)

5. [How to access the Odoo Database management interface](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-access-the-odoo-database-management-interface) - [Youtube Tutorial - How to access the Odoo Database management interface](https://youtu.be/JIRcz1WDLT0)

6. [How to connect to Odoo using pgadmin4](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-connect-to-odoo-using-pgadmin4) - [Youtube Tutorial - How to connect to Odoo using pgadmin4](https://youtu.be/afuB8wnozo8)

7. [Youtube Tutorial - What to do if you forget the admin password in Odoo 13](https://youtu.be/C3PGUApVHzM)

8. [How to enable Logging in Odoo 13](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-enable-logging-in-odoo-13) - [Youtube Tutorial - How to enable Logging in Odoo 13](https://youtu.be/TwgPfdvuvxQ)

9. [How to install addons that require Python packages in Odoo 13](https://github.com/twtrubiks/odoo-docker-tutorial#how-to-install-addons-that-require-python-packages-in-odoo-13) - [Youtube Tutorial - How to install addons that require Python packages in Odoo 13](https://youtu.be/CwaNdXV_2SY)

10. [Odoo 13 - How to create your own Docker Odoo image](https://github.com/twtrubiks/odoo-docker-tutorial#odoo-13---how-to-create-your-own-docker-odoo-image) - [Youtube Tutorial - Odoo 13 - How to create your own Docker Odoo image](https://youtu.be/n8n1nJyw9ZM)

11. [Odoo 13 - How to restore Odoo DB and filestore via CLI](https://github.com/twtrubiks/odoo-docker-tutorial#odoo-13---how-to-restore-odoo-db-and-filestore-via-cli) - [Youtube Tutorial - How to restore Odoo DB and filestore via CLI](https://youtu.be/1ng4_xP2e1c)

12. [Odoo - How to understand ORM RAW SQL through log_level](https://github.com/twtrubiks/odoo-docker-tutorial#odoo---how-to-understand-orm-raw-sql-through-log_level) - [Youtube Tutorial - How to understand ORM RAW SQL through log_level](https://youtu.be/sZWFGf23gWc)

## vscode debug odoo docker

Please refer to the [vscode_debug_docker_odoo17 branch](https://github.com/twtrubiks/odoo-docker-tutorial/tree/vscode_debug_docker_odoo17)

## Further Reading

[How to set up an Odoo development environment - Odoo 13 - from scratch](https://github.com/twtrubiks/odoo-development-environment-tutorial)

[Step-by-step guide to writing Odoo addons - Advanced](https://github.com/twtrubiks/odoo-demo-addons-tutorial)

## Introduction

What is Odoo, can you eat it :question:

[Odoo](https://www.odoo.com/) is an ERP system written in Python.

Odoo has two versions:

The free community edition [Odoo-Community](https://www.odoo.com/page/community), and the paid enterprise edition Odoo-Enterprise.

The functional differences between Odoo-Community and Odoo-Enterprise can be seen [HERE](https://www.odoo.com/page/editions).

As for why there is an Odoo-Enterprise, the reason is simple: to support the engineers :smile:

If you are interested, you can check out Odoo's articles on their website, which explain that this decision was made because the company was on the verge of failure.

The source code for Odoo Community is available on GitHub, you can find it here [odoo](https://github.com/odoo/odoo).

If you look at its branches, you will find many versions: Odoo 8, Odoo 9, Odoo 10,

Odoo 11, the latest Odoo 12, and the upcoming Odoo 13.

Let's keep the introduction brief for now and take a look at what Odoo is :satisfied:

## Tutorial

To quickly set up the environment, we will use Docker.

However, if you want to develop addons, I would recommend setting up Odoo from the source code.

If there's interest, I can make a video tutorial on this topic later :smirk:

If you don't know Docker, you can refer to my previous tutorial [Docker Beginners Guide - From Scratch](https://github.com/twtrubiks/docker-tutorial).

For Odoo's Docker images, I use [odoo docker](https://hub.docker.com/_/odoo).

First, let's look at `docker-compose.yml`

```yml
version: '3.5'
services:
  web:
    image: odoo:12.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    # command:
    #   odoo -r odoo -w odoo -i addons -d odoo
  db:
    image: postgres:10.9
    # ports:
    #   - "5432:5432"
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata

volumes:
  odoo-web-data:
  odoo-db-data:
```

There are two main services.

Let's start with `web`, which is the main Odoo service. Here we use `odoo:12.0`. By the way,

Odoo is built daily at [Odoo Nightly builds](http://nightly.odoo.com).

You can check the [odoo-docker-github](https://github.com/odoo/docker) to see which date's Odoo source code was used to build this image.

If you think it's too old, you can use its Dockerfile to build a newer version of Odoo. I can teach you how to do this in the future if there's interest :smirk:

`odoo-web-data:/var/lib/odoo` in volumes stores Odoo's data.

`./config:/etc/odoo` in volumes is for Odoo's configuration settings (which will be explained later).

`./addons:/mnt/extra-addons` in volumes is for Odoo's third-party addons.

I've placed a `manay-addons-folder` file in the [addons](https://github.com/twtrubiks/odoo-docker-tutorial/tree/master/addons) directory to indicate that this is where addons should be placed.

(Because GitHub doesn't allow empty directories :wink:)

Let me explain here, Odoo is actually composed of many addons.

You can see them here [https://github.com/odoo/odoo/tree/12.0/addons](https://github.com/odoo/odoo/tree/12.0/addons).

Each folder is an addon. Of course, existing addons may not meet all needs.

This is where customization and third-party addons come in.

You might ask, besides the native addons, where can I find third-party addons :question:

I'll provide a few places for reference, mainly two: first is [OCA](https://github.com/OCA), which has a ton of addons.

The second is [Odoo Apps](https://www.odoo.com/apps/modules), which has both free and paid addons.

This article won't cover how to write your own addon, but I may make a video on it in the future :smirk:

The `odoo -r odoo -w odoo -i addons -d odoo` in `command` is commented out.

The reason is that it's not needed. When would you need it :question:

When you need to install or update addons via the command line, you can write it like this.

For an explanation of Odoo's command line, refer to [Command-line interface:odoo-bin](https://www.odoo.com/documentation/12.0/reference/cmdline.html).

(Normally you would use `odoo-bin`, but in Docker, `odoo` is sufficient)

Now let's look at [odoo.conf](https://github.com/twtrubiks/odoo-docker-tutorial/tree/master/config)

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo
```

`addons_path` is the location of the addons. Usually, there are many, separated by commas.

`data_dir` stores the odoo folder.

There are many other parameter settings here (skipped for now) :smirk:

Next, let's look at `db`.

Odoo uses PostgreSQL. The `ports` section is commented out, which means it cannot be accessed from the local machine.

(If you don't understand why commenting out `ports` prevents local connection,

you can refer to [docker-compose ports vs expose difference](https://github.com/twtrubiks/docker-tutorial#docker-compose-ports-%E5%92%8C-expose-%E5%B7%AE%E7%95%B0))

If you want to connect to PostgreSQL with a tool like pgadmin4, please uncomment it so you can connect to the database.

(pgadmin4 is a free tool for connecting to PostgreSQL, you can refer to [Quickly set up pgadmin4 with Docker](https://github.com/twtrubiks/docker-pgadmin4-tutorial))

## Running Odoo

```cmd
docker-compose up
```

Then you can browse to [http://localhost:8069](http://localhost:8069).



You should see the following image.



After setting it up, please wait patiently.



## How to enable Odoo developer mode in Odoo 12

* [Youtube Tutorial - How to enable Odoo developer mode in Odoo 12](https://youtu.be/fUtqWQHbt1I)

In Odoo, enabling developer mode is very important :thumbsup:

It can help you with many things. So how do you enable it :question:

Click on Settings



You will see two options on the right:

Activate the developer mode -> I usually choose this one.

Activate the developer mode (with assets) -> This is for front-end debugging.



After clicking it, you will notice that `?debug` is added to the URL.



You will eventually find that you can enter debug mode by simply adding `?debug` after the `web` string in the URL.

If you're still feeling lazy, there are browser extensions to help,

like Odoo Debug for Google Chrome, and there are similar ones for Firefox.

## How to install third-party addons

* [Youtube Tutorial - How to install addons in Odoo](https://youtu.be/aN35xlUBKwc)

Using the [Row Number in tree/list view](https://apps.odoo.com/apps/modules/12.0/rowno_in_tree/) addon for testing.

Please remember to select the correct version, here we choose Odoo 12. After downloading, please unzip it.



Then put it in the `addons` folder.



I would recommend performing these two steps as well:

Give the addons folder maximum permissions (for Linux users).

```cmd
sudo chmod -R 777 addons
```

If you are not familiar with Linux commands, you can refer to [Notes on some Linux commands📝](https://github.com/twtrubiks/linux-note)



Then restart the server (this step is very important, sometimes you'll encounter strange issues if you don't restart :joy:)



Remember to enable debug mode, and click on Update Apps List.



Find the addon, the name of the addon is the name of the folder.



Click Install.

## How to access the Odoo Database management interface

* [Youtube Tutorial - How to access the Odoo Database management interface](https://youtu.be/JIRcz1WDLT0)

On the Odoo login page, there is a Manage Databases link at the bottom.



Click it and you will see the image below.



The URL is [http://0.0.0.0:8069/web/database/manager](http://0.0.0.0:8069/web/database/manager), you can also enter the URL directly.

Here you can manage Odoo databases, including creating, deleting, restoring, and backing up.



Have you noticed Set Master Password :question:

(See [Master Password](https://www.pgadmin.org/docs/pgadmin4/development/master_password.html))

It's actually an extra layer of protection :relieved:

If we set it, any operation on the page will require a password.

This Set Master Password can also be set in `odoo.conf`.

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo

admin_passwd = 666666
; list_db = False
```

(`;` represents a comment)

It will take effect after restarting.

For example, if I want to delete a DB, it will ask for your `admin_passwd`.



You might ask me, isn't it dangerous if any user can access it :question:

(Of course it's dangerous :joy:)

So this page can be disabled (by setting `list_db` to `False`).

```conf
[options]
addons_path = /mnt/extra-addons
data_dir = /var/lib/odoo

admin_passwd = 666666
list_db = False
```

This will prevent access to this page.



But this way, even the admin can't access this page :scream:

So a better way is to use Nginx to block the URL :smile:

(Set it so only specific IPs can access the page)

## How to connect to Odoo using pgadmin4

* [Youtube Tutorial - How to connect to Odoo using pgadmin4](https://youtu.be/afuB8wnozo8)

If you don't know what pgadmin4 is and how to install it, you can refer to my previous article.

[Quickly set up pgadmin4 with Docker and how to install pgadmin4 on Ubuntu locally](https://github.com/twtrubiks/docker-pgadmin4-tutorial)

Remember to open the DB port.



Enter the connection information (if you use my example, it will all be `odoo`).



After connecting, you will see many DBs, for example, I have created `odoo` and `odoo1`.



Find the table.



Use `expense` to test, modify the `name` field.



The data on the page also changes.



## How to enable Logging in Odoo

[Youtube Tutorial - How to enable Logging in Odoo 13](https://youtu.be/TwgPfdvuvxQ)

Please refer to the official documentation [odoo logging](https://www.odoo.com/documentation/13.0/reference/cmdline.html#logging)

Generally, logging is displayed in `stdout` (usually your terminal).

Note, this includes logs from both `web` and `db`.



But what if I want to save the logs, perhaps for analysis with ELK later :question:

First, go to [odoo.conf](https://github.com/twtrubiks/odoo-docker-tutorial/blob/master/config/odoo.conf) and add

```conf
......

logfile = /var/log/odoo/odoo.log
; critical, error, warn, debug
log_level = info
```

For easier viewing of the file, the `.yml` file also needs to be slightly modified.

```yml
version: '3.5'
services:
  web:
    image: odoo:12.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
      - ./odoo-log-data:/var/log/odoo # <----here
```

Sync the `/var/log/odoo` folder to the local `./odoo-log-data`.

And another important point, please give permissions to your `./odoo-log-data` :exclamation: :exclamation:

Otherwise, you will find that you can't write data to it :joy:

The method is also simple, just use the following command.

```cmd
sudo chmod -R 777 odoo-log-data
```

After restarting, you will find that your terminal only has db logs.



And the odoo logs are saved in `./odoo-log-data`.



You can also use `tail -f odoo-log-data/odoo.log` to keep track of the logs.

## How to install addons that require python packages in Odoo 13

[In preparation - Youtube Tutorial - How to install addons that require python packages in Odoo 13](https://youtu.be/CwaNdXV_2SY)

In Odoo, there are a ton of addons to play with, but I'm sure everyone has seen the following image.



I will use two addons to explain (and prevent) this problem.

Situation 1 (occasional).

Look at the [auto_backup](https://apps.odoo.com/apps/modules/13.0/auto_backup/) addon.

In its `auto_backup/models/db_backup.py`, you will see the following image.



This means you are missing the `paramiko` package.

This situation is harder to prevent unless you go in and check every `.py` file (they are usually at the top).

Situation 2 (common).

Look at the [report_xlsx](https://apps.odoo.com/apps/modules/13.0/report_xlsx/) addon.

In its `report_xlsx/__manifest__.py`, you will see the following image.



This is a more common situation, where the required python packages are included in `external_dependencies`.

This makes it easier, before installing the addon, you just need to check `__manifest__.py`.

Now that we've covered both situations, let's explain how to solve them :smile:

It's actually very simple, just install the python package. Here's an example using Docker. If you're not using Docker,

you can just run `pip3 install xxx` in your environment.

For the Docker method, first, make sure the Docker Odoo container is running, then run `docker exec -it xxx bash` to enter the container.

Then run `pip3 install paramiko`.



Finally, restart Odoo and install `auto_backup`.

Friendly reminder, do not use `docker-compose down` (because it will rebuild the container, and the package you installed will be gone).

Please use `docker-compose stop`.

If you don't understand, please refer to the following.

[Difference between docker-compose up/down and restart](https://github.com/twtrubiks/docker-tutorial#docker-compose-updown-%E5%92%8C-restart-%E7%9A%84%E5%B7%AE%E7%95%B0)

Or watch the video explanation directly.

[Youtube Tutorial - Difference between docker-compose up/down and restart](https://youtu.be/nX-sbLPz-MU)

You might ask me, what if I want to create my own Odoo images :question: (because I will be installing certain specific addons)

Let's continue to introduce this part below :satisfied:

## Odoo 13 - How to create your own Docker Odoo image

[Youtube Tutorial - Odoo 13 - How to create your own Docker Odoo image](https://youtu.be/n8n1nJyw9ZM)

The [docker-odoo](https://github.com/odoo/docker) repo is used.

Please see the [Dockerfile](https://github.com/twtrubiks/odoo-docker-tutorial/blob/master/build-odoo-image/Dockerfile), the main thing to modify is here.



```text
ARG ODOO_RELEASE=20200121
ARG ODOO_SHA=cb0bcb5d239983468c2e3b3f7cf17f58df820b1c
```

Suppose there is a bug that was fixed on 20200301, but the official Docker image is only up to January, what should I do :question:

At this point, you have to build it yourself :laughing:

Modify `ODOO_RELEASE=20200301` and then you need to check the following website for `ODOO_SHA` in `odoo_13.0.20200301_amd64.changes`.

[http://nightly.odoo.com/13.0/nightly/deb/](http://nightly.odoo.com/13.0/nightly/deb/)



```text
ARG ODOO_RELEASE=20200301
ARG ODOO_SHA=c72896852e6a6aa730db366a96d2602d91317bfa
```

This is the main modification for the Odoo image. If you want your Odoo image to include certain python packages by default,

you can add them yourself. For example, to add `paramiko`.



Finally, remember to give the folder permissions, `sudo chmod -R 777 build-odoo-image`.

Switch to the directory and enter the build command (this will take a while :smirk:)

```cmd
docker build -t twtrubiks/odoo13:20200301 .
```

After the build is complete, if everything is normal, enter `docker images` and you should see the following image.



Then modify the image in your `docker-compose.yml` to your own and test it.

You can also enter the Odoo container yourself to confirm if `paramiko` is installed.

## Odoo 13 - How to restore Odoo DB and filestore via CLI

[Youtube Tutorial - How to restore Odoo DB and filestore via CLI](https://youtu.be/1ng4_xP2e1c)

I have previously taught everyone [How to access the Odoo Database management interface](https://github.com/twtrubiks/odoo-docker-tutorial#%E5%A6%82%E4%BD%95%E9%80%B2%E5%85%A5-odoo-database-%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A2).

But sometimes you need to restore using the CLI, so today I will teach you this part :smile:

This will be demonstrated directly using Docker.

First, because the filestore will be restored, please sync the `odoo-web-data` volume to your local machine.

The DB will also be restored, so please also sync the `odoo-db-tmp` volume to your local machine.

It is also recommended to give them permissions `sudo chmod -R 777 odoo-web-data odoo-db-tmp`.

If you are not clear about this, please refer to [Docker Beginners Guide - From Scratch](https://github.com/twtrubiks/docker-tutorial).

The docker yaml is modified as follows:

```yaml
version: '3.5'
services:
  web:
    image: odoo:13.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - ./odoo-web-data:/var/lib/odoo   # <<---------------
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
      - ./odoo-log-data:/var/log/odoo
  db:
    image: postgres:10.9
    ......
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
      - ./odoo-db-tmp:/var/tmp # <<---------------

volumes:
  #odoo-web-data:   # <<---------------
```

Restoring via CLI requires two steps.

First, restore the DB.

Second, restore the filestore (usually where images are stored).

Suppose you want to restore the `twtrubiks` DB (backed up directly from the Odoo manager).

Open this zip, and you will see these things.



Now let's proceed with the first step, restoring the DB, which is restoring the `dump.sql` file.

Put the `dump.sql` file into the `odoo-db-tmp` folder.

This way you can see it in the `/var/tmp` location in the DB container.

```cmd
docker exec -it CONTAINER su postgres
```



Create the DB.

```cmd
createdb DB_NAME -U odoo -W odoo
```



At this point, if you go into the Odoo manager, you will see an exclamation mark because there is no data in it yet.



Restore the DB.

```cmd
psql -U odoo -d DB_NAME < dump.sql
```



Wait for it to run smoothly, which means the restoration is complete.

`-d` represents the dbname.

`-U` represents the username.

`-W` represents the password.

`-h` represents the hostname (can be ignored).

You will find that the exclamation mark has disappeared :smile:



Although you can successfully enter Odoo, you will find that the images are all gone :expressionless:

(It's possible you might see the images here, which means those images are stored in the DB).

Odoo can choose to store images in the DB or in the filestore.



Second step, restore the filestore (usually where images are stored).



Directly copy the `filestore` folder from the zip into `odoo-web-data/filestore`.

Then change the name of the `filestore` folder to the name of the DB.



It is recommended to give it permissions `sudo chmod -R 777 odoo-web-data`, and restart Odoo.



This way you can see the images successfully.

(If you still can't see the images, please check if you missed any steps, or check permissions, or browser cache)



The whole process is a bit more complicated :smirk:

## Odoo - How to understand ORM RAW SQL through log_level

[Youtube Tutorial - How to understand ORM RAW SQL through log_level](https://youtu.be/sZWFGf23gWc)

Sometimes, you're always curious about what the actual RAW SQL executed by a certain ORM is :question:

So in Odoo, how should we view this value :question:

Please go to [config/odoo.conf](https://github.com/twtrubiks/odoo-docker-tutorial/blob/master/config/odoo.conf) and modify `log_level`.

```conf
log_level = debug_sql
```

(`log_level` has many options, for details, please refer to the official documentation [logging](https://www.odoo.com/documentation/14.0/developer/misc/other/cmdline.html#logging))

Next, it is recommended not to use the normal command to run Odoo, because it will run for a long time :disappointed_relieved:

Please use the Odoo shell, you can refer to the previous tutorial [shell](https://github.com/twtrubiks/odoo-demo-addons-tutorial#shell)

[Youtube Tutorial - Odoo shell basic tutorial - CRUD](https://youtu.be/kmbiT54hUkw)

After entering the shell, execute any ORM command, and you can see the corresponding RAW SQL :smile:



Furthermore, you can take this SQL and execute it anywhere, like pgadmin4, and the output will be the same.

## Afterword

This introduction to Odoo is a very basic one to give everyone a brief understanding. There are still many things to talk about, such as how to write addons,

how to extend existing addons, the Odoo architecture, how to use Odoo with Nginx, etc......

Almost all of these can be made into separate videos to introduce to everyone.

## Further Reading

[How to set up an Odoo development environment - Odoo 13 - from scratch](https://github.com/twtrubiks/odoo-development-environment-tutorial)

[Step-by-step guide to writing Odoo addons - Advanced](https://github.com/twtrubiks/odoo-demo-addons-tutorial)

## Reference

* [Odoo](https://www.odoo.com/)
* [odoo docker](https://hub.docker.com/_/odoo)

## Donation

The articles are my own original work after research and internalization. If they have helped you and you would like to encourage me, feel free to buy me a coffee :laughing:

ECPAY (no membership registration required)

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[Sponsor Payment](http://bit.ly/2F7Jrha)

O'Pay (membership registration required)



[Sponsor Payment](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)

## Sponsor List

[Sponsor List](https://github.com/twtrubiks/Thank-you-for-donate)

## License

MIT license

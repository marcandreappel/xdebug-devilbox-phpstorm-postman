# Use XDebug on a Devilbox with PhpStorm and Postman 

## Host OS

Tested on:
 - openSUSE 15.1 Leap
 - Fedora 30
 - Ubuntu 18.04.3 LTS
 - Pop!\_OS 19.10 (might apply to most recent Ubuntu derivates)
 - Windows 10 Pro

Check before setup:
 - Accept in the firewall settings the port 9000 TCP

Follow the steps in the [setup tutorial](SETUP.md) (optional).

## Docker 

Make sure to follow [the Docker setup step](SETUP.md#docker) in the setup tutorial.

## Devilbox
(&hellip;_for a multisite setup read this part of the [Devilbox documentation](https://devilbox.readthedocs.io/en/latest/configuration-files/env-file.html#host-path-httpd-datadir)_&hellip;)

```sh
cd /_devilbox_install_dir_/cfg/php-ini-X.X/
cp devilbox-php.ini-xdebug xdebug.ini
nano xdebug.ini
```

Change these settings:

```ini
xdebug.default_enable  = 1
xdebug.profiler_enable = 1

xdebug.remote_enable       = 1
xdebug.remote_autostart    = 0
xdebug.remote_handler      = dbgp
xdebug.remote_port         = 9000
xdebug.remote_connect_back = 1

xdebug.idekey     = PHPSTORM
xdebug.remote_log = /var/log/php/xdebug.log
```

See the [example .ini file](cfg-examples/php7.3-xdebug.ini).

Save and restart DevilBox (or use the [handy startup script](/example-files/dbox)):

```sh
cd /_devilbox_install_dir_
docker-compose stop
docker-compose rm -f
docker-compose up -d 
```

&hellip; or when not everything is necessary:

```sh
docker-compose up httpd php mysql -d
```

**If you get errors, check [troubleshooting DNS related startup errors](systemd-resolved.md).**

## PhpStorm

**Settings** / **Languages & Frameworks** / **PHP**

 - Set CLI Interpreter by clicking on **&hellip;** 
 - Add a new interpreter by clicking on **+** then choose _From Docker, Vagr&hellip;_
 - Tick Docker and add the Docker Server and then choose under _Image name_ : **devilbox/php-fpm:X.X-work&hellip;**

**Settings** / **Languages & Frameworks** / **PHP** / **Debug**

```
Xdebug
    Debug port: 9000 ☑️ Can accept external connections
    ☑️ Force break at first line when no path mapping specified
    ☑️ Force break at first line when a script is outside the project
```

**Settings** / **Languages & Frameworks** / **PHP** / **Debug** / **DBGp Proxy**

```
IDE key: PHPSTORM
Host:    0.0.0.0
Port:    9000
```

**Settings** / **Languages & Frameworks** / **PHP** / **Servers**

 - Add a new server **+**
 - Set the Name of the server
 - Set values of _Host_ to **localhost** : _Port_ to **80**  
 - ☑️ Use path mappings (&hellip;)
 
 | File/Directory | Absolute path on the server |
 | ----------- | ----------- |
 | /home/user/devel/htdocs | /shared/httpd/devel/htdocs |

 
## Postman

Under `Query Params` set the key: **XDEBUG_SESSION_START** and the value **PHPSTORM**
Click on the link `Cookies` (beneath the _Send_ button) and add a new cookie for the corresponding domain:

```cookie
XDEBUG_SESSION=PHPSTORM; path=/; domain=.yourlocaldomain.loc;
```

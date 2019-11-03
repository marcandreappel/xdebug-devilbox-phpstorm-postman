[<< Return to the tutorial](README.md)

# Troubleshooting Auto DNS and systemd-resolved

## ERROR: for devilbox_bind_1 Cannot start service bind: (&hellip;) address already in use

This error might occur with the `cytopia/bind` container when using ``docker-compose up -d``.

### Ubuntu & derivates

_The following was tested on Ubuntu 19.10_

 Delete the systemd-resolved stub-file first: 
 
 ```sh
 sudo rm /etc/resolve.conf
 ```
 
 Symlink the systemd-resolved file: 
 
 ```sh
 sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
 ```
 
 Edit the systemd-resolved configuration:
 ```sh
 sudo nano /etc/systemd/resolved.conf
 ```
 Add the following into it:
 
 ```conf
 [Resolve]
 DNS=127.0.0.1 8.8.8.8
 ```
 
 Restart systemd-resolved to validate the configuration:

 ```sh
 sudo systemctl restartsystemd-resolved.service
 ```
 Save somewhere the [dbox tool](example-files/dbox), change the path to your devilbox installation and set it excutable `chmod u+x dbox.sh`

### Other systems

The docs might give [the solution here](https://devilbox.readthedocs.io/en/latest/howto/dns/add-custom-dns-server-on-linux.html) (check `/etc/resolv.conf` to get your running network manager / DNS resolver).

# Troubleshooting systemd-resolved service and Devilbox auto-dns

## "$ ERROR: for devilbox_bind_1 Cannot start service bind: (&hellip;) address already in use"

This error might occur with the `cytopia/bind` container when using ``docker-compose up -d``.

### Ubuntu & derivates

 - Delete the stub-file first: `/etc/resolve.conf`
 - Symlink the systemd-resolved file: `sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf`
 - Edit the systemd-resolved configuration `sudo nano /etc/systemd/resolved.conf` by adding `DNS=127.0.0.1 8.8.8.8` beneath **[Resolve]**
 - Restart systemd-resolved `sudo systemctl restartsystemd-resolved.service` to actualize the configuration
 - Add somewhere the [dbox.sh](https://gist.github.com/marcandreappel/a51f5fcdbe437d9cad0e4fee05e798d0) script and set it excutable `chmod u+x dbox.sh`

### Others

The docs might give [the solution here](https://devilbox.readthedocs.io/en/latest/howto/dns/add-custom-dns-server-on-linux.html) (check `/etc/resolv.conf` to get your running network manager / DNS resolver).

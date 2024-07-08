# Debian Wheezy

## Deluge Launchpad PPA
One of the simplest methods to getting the latest Deluge version is from the Deluge Launchpad PPA. Ubuntu Precise release repo will be used as this is based upon Debian Wheezy. 

Add the deluge-team PPA:

```
add-apt-repository 'deb http://ppa.launchpad.net/deluge-team/ppa/ubuntu precise main'
```

Refresh apt:

```
apt-get update
```

Install Deluge:

```
apt-get install -t precise deluge-common deluged deluge-web deluge-console
```


Upgrading libtorrent:

 http://dev.deluge-torrent.org/wiki/Building/libtorrent#UbuntuDebian
 
 There is a boost issue using libtorrent from the PPA so building from source is the only viable option.
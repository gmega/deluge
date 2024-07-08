# Debian Lenny

Installing a recent version of deluge (1.3.0) on Lenny can be done relatively easily. 

However, it involves fetching a few packages from testing so be careful. Though this usually can be done without causing any problems it might interfere with other stuff on your system.

**You have been warned.**

The first thing we need to do is add testing to `/etc/apt/sources.list`:

```
deb http://ftp.nl.debian.org/debian/ squeeze main contrib non-free
deb-src http://ftp.nl.debian.org/debian/ squeeze main contrib non-free
```

Then we need to tell `apt` that even though we like testing, we do not want our packages taken form testing unless it's explicitly being told to do so (otherwise apt will just upgrade you to testing/Squeeze).
Create the file `/etc/apt/preferences`:

```
Package: *
Pin: release a=stable
Pin-Priority: 700

Package: *
Pin: release a=testing
Pin-Priority: 650
```

Now refresh apt:

```
apt-get update
```

Now we're going to have to use `aptitude` for it to automatically resolve some dependency troubles as we're going to need to install newer version of some things that already are on your system:

```
aptitude install python-libtorrent

[a lot of output from aptitude now follows]

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Reading extended state information      
Initializing package states... Done
Writing extended state information... Done
Reading task descriptions... Done         
The following packages are BROKEN:
  libtorrent-rasterbar5 python-libtorrent 
The following NEW packages will be installed:
  file{a} libboost-filesystem1.42.0{a} libboost-python1.42.0{a} libboost-system1.42.0{a} libboost-thread1.42.0{a} libdb4.5{a} libmagic1{a} libsqlite3-0{a} mime-support{a} 
  python{a} python-minimal{a} python2.5{a} python2.5-minimal{a} 
0 packages upgraded, 15 newly installed, 0 to remove and 0 not upgraded.
Need to get 7962kB of archives. After unpacking 28.2MB will be used.
The following packages have unmet dependencies:
  libtorrent-rasterbar5: Depends: libgeoip1 (>= 1.4.7~beta3+dfsg) but it is not installable
                         Depends: libssl0.9.8 (>= 0.9.8m-1) but 0.9.8g-15+lenny8 is installed.
                         Depends: libstdc++6 (>= 4.4.0) but 4.3.2-1.1 is installed.
  python-libtorrent: Depends: libssl0.9.8 (>= 0.9.8m-1) but 0.9.8g-15+lenny8 is installed.
                     Depends: libstdc++6 (>= 4.4.0) but 4.3.2-1.1 is installed.
                     Depends: python-support (>= 0.90.0) but it is not installable
The following actions will resolve these dependencies:

Install the following packages:
gcc-4.4-base [4.4.5-2 (testing)]
geoip-database [1.4.7~beta6+dfsg-1 (testing)]
libgeoip1 [1.4.7~beta6+dfsg-1 (testing)]
python-support [1.0.10 (testing)]

Upgrade the following packages:
libssl0.9.8 [0.9.8g-15+lenny8 (stable, stable, now) -> 0.9.8o-2 (testing)]
libstdc++6 [4.3.2-1.1 (stable, now) -> 4.4.5-2 (testing)]

Score is 39

Accept this solution? [Y/n/q/?] Y

The following NEW packages will be installed:
  file{a} gcc-4.4-base{a} geoip-database{a} libboost-filesystem1.42.0{a} libboost-python1.42.0{a} libboost-system1.42.0{a} libboost-thread1.42.0{a} libdb4.5{a} libgeoip1{a} 
  libmagic1{a} libsqlite3-0{a} libtorrent-rasterbar5{a} mime-support{a} python{a} python-libtorrent python-minimal{a} python-support{a} python2.5{a} python2.5-minimal{a} 
The following packages will be upgraded:
  libssl0.9.8 libstdc++6 
2 packages upgraded, 19 newly installed, 0 to remove and 0 not upgraded.
Need to get 14.5MB of archives. After unpacking 34.8MB will be used.
Do you want to continue? [Y/n/?] Y
[..]
```
Basically, `aptitude` found a solution to our problem, it's going to upgrade the necessary packages with packages from testing as to meet the dependencies for `python-libtorrent` which we're going to need later.

Now it's time to add yet another repository with the packages necessary to get deluge, so add the following to `/etc/apt/sources.list`:

```
deb http://ppa.launchpad.net/ferramroberto/linuxfreedomlucid/ubuntu lucid main 
deb-src http://ppa.launchpad.net/ferramroberto/linuxfreedomlucid/ubuntu lucid main
```

Then, lets fetch and add the GPG key for that repository in order to be able to verify the downloaded packages:

```
apt-key adv --recv-keys --keyserver pgp.surfnet.nl 249AD24C
```

Refresh apt:

```
apt-get update
```

Install deluge, for example:

```
apt-get install -t lucid deluge
```

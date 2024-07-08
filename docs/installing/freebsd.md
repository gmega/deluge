# Installing Deluge On FreeBSD

Deluge (1.3.x) is available in [Freshports](http://www.freshports.org/net-p2p/deluge/), with dependencies correctly identified and also stable. 

## Install package with GTK2 client

```
pkg add deluge
```

## Install package for headless setups

```
pkg add deluge-cli
```


## Install from Ports

For desktop use (X11):

```
cd /usr/ports/net-p2p/deluge && make install clean
```

Or using portmaster:

```
portmaster net-p2p/deluge
```

*Note: The above commands should work correctly to install UI and Daemon components on FreeBSD 7.2 or later, installing the Python/Boost dependencies as required.*

---

For headless setups (no X11):

```
cd /usr/ports/net-p2p/deluge && make WITHOUT_X11=yes install clean
```
and disable GTK2 in the port configuration options.

*Note:  Option WITHOUT_X11="YES" can also be set globally (for all port builds) in /etc/make.conf. As of (at least) FreeBSD-9.1, WITHOUT_X11 is deprecated and use of OPTIONS_UNSET=X11 in make.conf is advised instead (and should probably be preferred for versions that support it.*


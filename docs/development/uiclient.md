# Overview

Deluge is separated into two distinct portions, the daemon and the client, and as such, there needs to be a way for these components to communicate.  The deluge.ui.client module is designed to facilitate this communication for the various User Interfaces.  The client module hides the DelugeRPC protocol from us and presents with an easy to use API to access Deluge daemons.

If you are not familiar with Twisted Deferred objects, then I would strongly suggest [reading about them](http://twistedmatrix.com/projects/core/documentation/howto/defer.html) before getting started.

# Writing a simple client

Since I like to learn by example, I am going to start right away with a basic Deluge client.

```python

# Import the client module
from deluge.ui.client import client
# Import the reactor module from Twisted - this is for our mainloop
from twisted.internet import reactor

# Set up the logger to print out errors
from deluge.log import setupLogger
setupLogger()

# Connect to a daemon running on the localhost
# We get a Deferred object from this method and we use this to know if and when
# the connection succeeded or failed.
d = client.connect()

# We create a callback function to be called upon a successful connection
def on_connect_success(result):
    print "Connection was successful!"
    print "result:", result
    # Disconnect from the daemon once we successfully connect
    client.disconnect()
    # Stop the twisted main loop and exit
    reactor.stop()

# We add the callback to the Deferred object we got from connect()
d.addCallback(on_connect_success)

# We create another callback function to be called when an error is encountered
def on_connect_fail(result):
    print "Connection failed!"
    print "result:", result

# We add the callback (in this case it's an errback, for error)
d.addErrback(on_connect_fail)

# Run the twisted main loop to make everything go
reactor.run()

```

All this client script will do is try to connect to a daemon running on localhost and then disconnect from it right away.  The script is pretty useless, but it shows how things are done in an asynchronous matter and this is an important concept for developing more complex interfaces.

So now that we've got a basic script to get us connected to a daemon, let's extend it a bit to do something useful.  We'll start by adding a remote procedure call in the **on_connect_success()** function.

```python

def on_connect_success(result):
    print "Connection was successful!"
    def on_get_config_value(value, key):
        print "Got config value from the daemon!"
        print "%s: %s" % (key, value)
        # Disconnect from the daemon once we've got what we needed
        client.disconnect()
        # Stop the twisted main loop and exit
        reactor.stop()
    
    # Request the config value for the key 'download_location'    
    client.core.get_config_value("download_location").addCallback(on_get_config_value, "download_location")

```

Ok! We now should be getting a print out of the *download_location* config value.  You'll notice that any RPC method returns a Deferred object, just like the **client.connect()** method.  Since the **core.get_config_value()** method only returns the value we are passing the *key* to the callback function too.  So in the **on_get_config_value()** callback, the first argument is the return value from the daemon and the second is from our **addCallback()** call.

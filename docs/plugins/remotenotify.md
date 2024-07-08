# RemoteNotify

**RemoteNotify** is a 1.1.x plugin to Notify you when Torrents have completed by sending an Instant Message to a configured Jabber ID. Also, it allows limited Remote Control of the Torrents with easy Jabber message commands.

**RemoteNotify** has core and webui components but no gtk.

## Features

* Sends an Instant Message upon completion of any Download.
* The status of all the downloads is updated every 10 seconds in the contact "away/status message bubble".
* Only Buddies(Authorized JIDs) can send Deluge Commands to Bot.
* It also has Mail Notifications implemented as a Core Plugin, since the included Deluge Mail Notification works only with the GTK UI on.
* Status, Adding, Removing of Torrents by sending IM message commands.

## IM Commands

* status - Prints current status information about all downloading torrents
* add [URL of Torrent] - Gets Torrent File from URL and starts downloading
* del [Id of Torrent] - Deletes Torrent File with given Id (hash Id - the one shown in the *status* command)
* help - This command

## Requirements

* [TwistedWords](http://twistedmatrix.com/trac/wiki/TwistedWords)

## Installation & Configuration

### Compile & Install Plugin

1. Compile code

```
python setup.py bdist_egg
```
2. Copy plugin into deluge plugin directory ( usually ~/.config/deluge/plugins )

```
cp dist/RemoteNotify-0.1-py2.5.egg ~/.config/deluge/plugins/
```

### Configure Jabber Account for the Bot

1. Create a Jabber account for your Bot ( http://register.jabber.org )
2. Log on with your favourite IM Chat Application on both your and the Bot's accounts and manually authorize the Bot and viceversa. ( This isn't done by the plugin! ). Both Accounts should see the other one as online. Only Buddies(Authorized JIDs) can send Deluge Commands to Bot

### Configure Mail Account for the Bot

1. Create or use some existing mail account that offers an smtp service. I used gmail ( http://mail.google.com/mail/signup ).

### Start & Configure the Plugin

1. Start deluged & deluge webUI
2. Enable RemoteNotify plugin ( using webui at http://yourdelugedomain/config/plugins </ the ajax theme doesn't have this )
3. Enter The Account & Password for your Bots jabber and Email account & save. ( http://yourdelugedomain/config/remotenotify )
4. Enter Notifier Email SMTP server. (For gmail use *smtp.gmail.com* )
5. Restart deluged. ( Sadly, this is necessary because of a weird Twisted Words bug )
6. Have phun chatting with your Deluge Jabberbot

Any questions/bugs should be sent to the plugin forum:
http://forum.deluge-torrent.org/viewtopic.php?f=9&t=18465

## Change Log

* 0.1 - Initial release
* 0.2 - Deadlock bugfix when notifying user of finished download.

## Download

See Attachment
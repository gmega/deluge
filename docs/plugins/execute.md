## Execute
The **Execute** plugin gives you the option to run your own scripts (e.g. `Bash`, `Python` or any other shell) triggered with different events in Deluge. At this moment only the events **Added** (when a torrent is added to deluge) and **Complete** (when a torrent has finished downloading) are supported. More events will be added in the future.

The script to be executed will be supplied with three torrent variables:

1. ID (Hash)
2. Name
3. Folder:
   * *Torrent Added:* 'Download'
   * *Torrent Complete:* 'Download' or 'Move Completed', if setup.

## Configuration

Enable the plugin in the Plugins menu in Preferences. 

***Important: After enabling this plugin Deluge will require restarting for it to work properly.***

The events Torrent Complete and Torrent Added can be selected and the full path to the script entered (e.g. /home/user/execute_script)

![](http://forum.deluge-torrent.org/download/file.php?id=3263)

Make sure the script is set to executable: 

```
chmod u+x /home/user/execute_script
```
Also ensure that it is accessible by the user used to run deluged.

Before adding to Deluge, scripts should be tested manually like so:

```
./execute_script "TorrentID" "Torrent Name" "Torrent Path"
```

## Script Examples
The following scripts can be used for testing purposes.

### Bash

```sh
#!/bin/bash
torrentid=$1
torrentname=$2
torrentpath=$3
echo "Torrent Details: " "$torrentname" "$torrentpath" "$torrentid"  >> /tmp/execute_script.log
```

The variables will show up in the log file: `/tmp/execute_script.log`

### Windows Batch
Save this with `.bat` extension such as `execute_example.bat`:

```bat
@echo off
set torrentid=%1
set torrentname=%2
set torrentpath=%3
@echo Torrent Details:  %torrentname% %torrentpath% %torrentid%  >> "%userprofile%\Desktop\execute_script.log"
```


The variables will show up in the log file on the user Desktop. 

### Perl

```perl
#!/usr/bin/perl
$torrentid = $ARGV[0];
$torrentname = $ARGV[1];
$torrentpath = $ARGV[2];
open(my $fh, '>>', '/tmp/execute_perl.log');
print $fh "Torrent Details: $torrentname $torrentpath $torrentid\n"
```

The variables will show up in the log file: `/tmp/execute_perl.log`

### Python

```python
#!/usr/bin/env python
from sys import argv
from syslog import syslog
syslog('deluge test: the script started running')
for arg in argv[1:]:
    syslog(arg)
```

The variables should show up in your syslog. 

```
tail /var/log/syslog
```

### Sendemail script
As a alternative to the notifications plugin you can send your e-mails through the execute script by using the program `sendemail`. By using `sendemail` it is possible to send e-mails to a smtp server all from one (command)-line.

To install sendemail on Ubuntu:

```
sudo apt-get install sendemail
```
The following command sends a e-mail:

```
sendemail -s smtp.myisp.com -t myemail@domain.com -f fromemail@domain.com -u "subject" -m "message"
```

The script should be named *torrent_added* and contain the following:

```sh
#!/bin/bash
torrentid=$1
torrentname=$2
torrentpath=$3

subject="Started download new torrent!"
message="$torrentname to $torrentpath"

echo -e `date`"::Started download torrent:$2 in: $3" with id:$torrentid >> ~/scripts.log
echo -e `sendemail -s smtp.myisp.com -t myemail@domain.com -f fromemail@domain.com -u "Started downloading $torrentname!" -m "Downloading $torrentname to: $torrentpath"` >> ~/scripts.log
```

Make sure that you substitute smtp.myisp.com, myemail@domain.com and fromemail@domain.com with your own data!

This script can be rewritten to be used with the complete event. This way you will get a e-mail when a torrent is started and finished.

You can also achieve the same outcome with a python script:

```python
#!/usr/bin/env python
import sys 
import smtplib

torrent_id = sys.argv[1]
torrent_name = sys.argv[2]
save_path = sys.argv[3]

#You can change these
from_addr = "fromemail@domain.com"
to_addr = "myemail@domain.com"
smtp_srv = "smtp.myisp.com"

subject = "Some subject"
message = """Put your message here

It can span multiple lines and can also contain information by specifying
%(torrent_id)s or %(torrent_name)s""" % { 
    'torrent_id': torrent_id,
    'torrent_name': torrent_name,
    'save_path': save_path
}

msg = "To:%s\nFrom:%s\nSubject: %s\n\n%s" % (to_addr, from_addr, subject, message)

smtp = smtplib.SMTP(smtp_srv)
smtp.sendmail(from_addr, to_addr, msg)
smtp.quit()
```

### Extract archives script

This script will search for archives in downloaded content and extract them. Actions are logged as entries in syslog.
It can be easily extended to support more compression and archive formats.

```sh
#!/bin/bash
formats=(zip rar)
commands=([zip]="unzip -u" [rar]="unrar -o- e")
extraction_subdir='extracted'

torrentid=$1
torrentname=$2
torrentpath=$3

log()
{
    logger -t deluge-extractarchives "$@"
}

log "Torrent complete: $@"
cd "${torrentpath}"
for format in "${formats[@]}"; do
    while read file; do 
        log "Extracting \"$file\""
        cd "$(dirname "$file")"
        file=$(basename "$file")
        # if extraction_subdir is not empty, extract to subdirectory
        if [[ ! -z "$extraction_subdir" ]] ; then
            mkdir "$extraction_subdir"
            cd "$extraction_subdir"
            file="../$file"
        fi
        ${commands[$format]} "$file"
    done < <(find "$torrentpath/$torrentname" -iname "*.${format}" )
done
```
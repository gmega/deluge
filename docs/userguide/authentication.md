## Authentication
The daemon looks at the file [<config folder>](/faq.md#where-does-deluge-store-its-settingsconfig)/auth for doing authentication.

The format of this file is straightforward, each line contains a username:password:level tuple in plaintext.

There should always be a 'localclient' entry for use by the UIs running locally by your user.

**Note:**
If you do not have an auth file in your config folder, first run the daemon to have it created for you.

Deluge 1.2.0 introduces different levels of authentication:

|            |             |
|------------|-------------|
| Level Name | Level Value |
| None | 0 |
| Read Only | 1 |
| Normal | 5 |
| Admin | 10 |

**Note:**
In Deluge 1.3.3 authentication levels don't do anything. In the future they will.
In git master there is a multiuser option that makes use of authentication levels.

**Example of an auth file:**

```
localclient:a7bef72a890:10
andrew:password:10
user3:anotherpass:5
```
**Example of adding a new user under Linux:**

```
echo "username:password:level" >> ~/.config/deluge/auth
```

From the GtkUI, you will have to add the host with a username and password, if you don't do this, you won't be able to connect to the host or tell if it's online.
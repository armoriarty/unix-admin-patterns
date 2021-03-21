# Postfix Setup

Postfix is an email server application that can send and receive emails.

## Setup

- Install postfix and some utility programs.

```bash
sudo apt install postfix mailutils
```

- Run a test (See test section below)

- Since you have not set up anything other than postfix we need to manually check the mail to ensure your test worked. The `mailutils` provides a `mail` program. Switch to the user that you sent mail to and use the `mail` command. A mailbox with your test email should appear. if not see the troubleshooting items at the end of the test section.

- Setting up maildir

    Mail is normally stored at `/var/spool/mail/<USER>`. This is not always ideal because of permissions, file locking, etc...  Maildir is a format that stores email in each user's home directory and solves a lot of the problems with the standard setup and enables [POP](pop.md)/[IMAP](imap.md).

    - Edit the postfix configuration file at `/etc/postfix/main.cf`
    - Modify or add the following lines to the configuration file:
        - `home_mailbox = Maildir/`
        - `mailbox_command = `
    - Restart postfix using the command `sudo systemctl restart postfix`

- Run through the e-mail test again

    - Once you have sent the email change your environment variable for `MAIL` to reflect your new maildir. Use the command `MAIL=/home/<USER>/Maildir` in the user's terminal. 

    - If you want to make it perminant then add the line `export MAIL=/home/<USER>/Maildir` to your .bashrc file.

    - Run the `mail` command like before to access your inbox.


Once you have completed maildir you are ready to move on to [POP](pop.md) and [IMAP](imap.md)


## Test Your setup

- Find your server's ip address or name 

    You can find your computer's ip address by using the command: `ip --br addr`. You should see output like

```bash
wlp2s0           UP             35.24.107.58/21 fe80::2226:648d:f9d1:9197/64
```

That `35.24.107.58` is your ip address. You can use that or you can find the actual url assigned to that up address using the command `nslookup <ip address>`. 

- begin sending the message using the command

```bash
telnet <url or ip address> 25 
```

- You should connect and see a prompt that looks similar to:

```bash
Trying 35.24.107.58...
Connected to 35.24.107.58.
Escape character is '^]'.
220 rvr35107jamrichwifi58.nmu.edu ESMTP Postfix (Ubuntu)
```

- If you see this then your are ready to move onto the next step. If not then you should check:
    - That the ip/url you typed in is correct
    - Firewalls are off
    - Log files on your server at `/var/log/mail.log`

- You need to manually type in the message format for your email to be processed correctly. The format is:

```
ehlo <ip address>
mail from: <name>@<server you want to appear from. This doesn't matter>
rcpt to: <name>@<server you want to send to. This DOES MATTER.>
data
Subject: <Your subject>

<Body of your email>
.
quit
```

You should send the email to a known email address that you have access to.

- The email should be sent in under a few minutes. Once you have verified the email arrived you can be confident your server is working. Congrats!

    If not consider:
    
    - That the ip/url you typed in is correct both for the email server and the `rcpt to:` field.
    - Firewalls are off
    - Are you trying to send emails into the NMU network? They are probably blocked at the school's network layer. Try sending emails between machines inside the network. (school email accounts are outside the network because google...)
    - Log files on your server at `/var/log/mail.log`


## References

- [Basic Postfix Setup](https://help.ubuntu.com/community/PostfixBasicSetupHowto)
# Φ-Math's website

![Phi-Math's logo. It is a white capital phi on a purple circle](static/favicon.ico)

## Prerequisites

- [Git](https://git-scm.com/downloads)
- [Hugo](https://gohugo.io/installation)

## Clone

We use `--recursive` (from most code editors, "Git Clone (Recursive)" as opposed
to "Git Clone") to ensure the theme subdmodule is cloned, too.

```
git clone --recursive git@github.com:phi-math-website/phi-math-website.git
```

## Test

If you leave this command running and modify content, your local test website
instance will automatically update.

```
hugo server -O
```

If you make changes to some configuration values, the instance might not update.
In this case. Stop and rerun the command instead.

## Build

Once you are satisfied with your modifications, run:

```
hugo
```

This will generate your website in a new `public/` folder. Next, we'll see how
to upload this to
[events.illc.uva.nl/Phi-MathReading](https://events.illc.uva.nl/Phi-MathReading).

## Deploy

### Prerequisites

- SSH, or any program like SFTP, SCP or RSync that can work over the SSH protocol.

### Asking for access

You can email webmaster [Marco Vervoort](https://www.illc.uva.nl/People/person/1566/Dr-Marco-Vervoort)
your UvAnetID (e.g., `74958495`) explaining that you are a new $\Phi$-Math board member. Marco will grant
you access. For the remainder of this guide, we will refer to your UvAnetID as `UVANETID` within
commands/configuration files.

### SSH setup

Logging in directly to the server with SSH is blocked by ICTS. Instead you log in via a 'gateway server'
with hostname `pascal.ic.uva.nl`. For testing you can log in with SSH using `ssh UVANETID@pascal.ic.uva.nl` and
then login by executing the command on this server `ssh UVANETID@goedel.fnwi.uva.nl`.

For practical use you can automate this 'log through' by making sure you have a file in your home directory
`.ssh/config` and adding a section in it

```
Host phimathproxy
  Hostname pascal.ic.uva.nl
  User UVANETID

Host phimath
  Hostname goedel.fnwi.uva.nl
  ProxyJump phimathproxy
  User UVANETID
```

This ensures that when you run the command `ssh phimath` on your own computer,
SSH will first log onto the gateway server and then do the 'log through'
automatically. And this will also configure other programs based on SSH (like
the aforementioned SFTP, SCP and Rsync) to use the same mechanism to access
`goedel.fnwi.uva.nl`.

Initially, this setup means that you have to enter your UvANetID password twice.
If you log in separately with SSH on `pascal.ic.uva.nl`(using `ssh
UVANETID@pascal.ic.uva.nl`) you can use the command `vi_authorized_keys` to add
a public SSH key (using the `vi` text editor from the terminal), so that you no
longer have to enter a password for the `pascal.ic.uva.nl` login. If you don't
have a public SSH key yet, you can create one by running the command on your own
computer (not the web server) `ssh-keygen` then two files will be created on
your own computer in the subdirectory '.ssh', '.ssh/id_rsa.pub' (the public SSH
key whose content you are using above) and '.ssh/id_rsa' (a private SSH key
that you should not share with others and that SSH uses when logging in).

#### On Linux systems

On Linux systems, if you automate the 'log-through' for SSH as described above,
the other programs based on SSH (like the aforementioned SFTP, SCP and Rsync)
will also use this configuration to access the server. So you can just run
things like:

```
rsync -r public/* phimath:/var/www/illc/events/Phi-MathReading/
```

#### On Windows systems (WinSCP users)

If you use Windows and the WinSCP file-transfer client to upload files, after
opening the program you should configure it by clicking 'New Connection' and
first specify

```
  Host name: goedel.fnwi.uva.nl
  User name: UVANETID
```

and then as an extra step click on the 'Advanced Settings' option, navigate to
'Connection >> Tunnel', and set

```
  [v] Connect through SSH tunnel
  Tunnel Host name:pascal.ic.uva.nl
  Tunnel User name: UVANETID
```

You will need to enter your password twice, once for the tunnel and once for the
server itself, unless you already have configured passwordless login on
`pascal.ic.uva.nl`.

Other file-transfer clients may or may not have similar functionality.

#### On MacOS systems

On Mac systems, FileZilla doesn't support setting jump-hosts, but CyberDuck
does.

### Website update

After logging in, the site files may be found in the filesystem at
`/var/www/illc/events/Phi-MathReading`. You can modify them there.

Finally, Marco has a request that he would ask us to keep in mind when managing
the site files: can we make sure that all files and subdirectories are created
with 'group-writeable' permissions, and all subdirectories also with the
'set-group-id' flag? This ensures that files and directories belong to the user
group `www-illc-phi-mathreading-science` and are editable by all members of this
group. To do it manually, we can use the commands

```
chmod -R g+w /var/www/illc/events/Phi-MathReading/*
chgrp -R www-illc-phi-mathreading-science /var/www/illc/events/Phi-MathReading
find /var/www/illc/events/Phi-MathReading/*/ -type d -exec chmod g+s {} +
```

If we forgot this step, the other $\Phi$-Math board member will be unable to
edit these files!

For general hosting Marco has a few more requests, but those are not relevant
for $\Phi$-Math, as we are using the 'create static site on your own computer
and then upload the pages' approach. If we ever want to switch, we should
contact Marco first.

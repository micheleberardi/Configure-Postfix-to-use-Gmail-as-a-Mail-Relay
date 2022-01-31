# Configure-Postfix-to-use-Gmail-as-a-Mail-Relay
Configure Postfix to use Gmail as a Mail Relay

# Install Required Software
```yum update && yum install postfix mailx cyrus-sasl cyrus-sasl-plain```

# Postfix configuration 

files reside in the directory /etc/postfix. 

Create or edit the password file:

``` vim nano /etc/postfix/sasl_passwd ```

# Add the line:

``` [smtp.gmail.com]:587    username@gmail.com:password ```

Save and close the file. 

Your Gmail password is stored as plaintext, so make the file accessible only by root:

``` chmod 600 /etc/postfix/sasl_passwd ```

# Configure Postfix

Edit the main Postfix configuration file:

```
vim /etc/postfix/main.cf

```

# Add or modify the following values:

```
myhostname = rc.domain.com

mailbox_size_limit = 5120000000

# sets gmail as relay
relayhost = [smtp.gmail.com]:587

#  use tls
smtp_use_tls=yes

# use sasl when authenticating to foreign SMTP servers
smtp_sasl_auth_enable = yes

# path to password map file
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd

# list of CAs to trust when verifying server certificate
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

# eliminates default security options which are imcompatible with gmail
smtp_sasl_security_options =

#debug_peer_list=smtp.gmail.com
#debug_peer_level=3

smtp_use_tls=yes

smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

Save and close the file.

#  Process Password File

```
postmap /etc/postfix/sasl_passwd

# Restart Postfix

```
systemctl restart postfix.service

```

# Send a test email

```
echo "Test mail from postfix" | mail -s "Test Postfix" name@domain.com
```

# DONE

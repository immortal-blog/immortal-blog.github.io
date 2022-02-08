# Send email with postfix on Ubuntu

My method referenced many tutorials, fixed the errors and worked successfully.

## Install mailutils

```bash
sudo apt install mailutils
```

## Configure username and password

```bash
sudo nano /etc/postfix/sasl_passwd
```

Input SMTP server, account and password.

```
[smtp.126.com]:465 username@126.com:password
```

The password of the 126 mailbox needs to generate an authorization code in the settings.  
Then run postmap.

```bash
sudo postmap /etc/postfix/sasl_passwd
```

## Map sender

By default, postfix uses your username@hostname (the one you can see in the title bar of the Ubuntu Console window) to send emails. Such emails will be rejected by the smtp server (553 Mail from must equal authorized user), so you need to put it mapped to your real email address.

```bash
sudo nano /etc/postfix/generic
```

Write mapping.

```
root@ubuntu     username@126.com
```

Then run postmap.

```bash
sudo postmap /etc/postfix/generic
```

## Modify the configuration file

```bash
sudo nano /etc/postfix/main.cf
```

edit `relayhost`.

```
relayhost = [smtp.126.com]:465
```

Add the following configuration at the end of the file.

```
# enable SASL authentication
smtp_sasl_auth_enable = yes

# disallow methods that allow anonymous authentication.
smtp_sasl_security_options = noanonymous

# where to find sasl_passwd
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd

# where to find generic
smtp_generic_maps = hash:/etc/postfix/generic

# Enable STARTTLS encryption
smtp_use_tls = yes

# where to find CA certificates
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

# Enable tls encryption
smtp_tls_wrappermode = yes
smtp_tls_security_level = encrypt
```

## Send email

Then restart postfix and you can send emails.

```bash
sudo service postfix restart
echo "test" | mail -s "hello" target@126.com
```

## View running log

If there is no response after running, see if there is any error in the log, so as to search and solve the problem in a targeted manner. You can open a separate terminal and place it next to it to view the log in real time.

```bash
tail -f /var/log/mail.log
```

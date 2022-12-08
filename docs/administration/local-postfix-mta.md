---
parent: Administration Tasks
title: Local Postfix MTA
nav_order: 6
---
# Working with the local Postfix MTA

## Background

The Yoda External User Service (EUS) and the main ruleset send email messages to users. In the standard
configuration, the EUS and ruleset send messages directly to a relay server, which would typically
be your organization's email server.

Alternatively, the EUS and ruleset can be configured to send messages to a local Postfix Mail Transfer
Agent. Postfix then takes care of delivering the messages to a relay host, such as your organization's
email server.

## Advantages

The main advantages of using Postfix rather than sending email messages directly to the organizational
email server are:
* If Yoda delivers emails directly to the organizational email server, any connection or availability issues
  can result in user actions failing. For example, if a Yoda user creates a new external account by adding
  an email address of a new user to a research group, the user will see an error message if the email server
  is not available. If Yoda uses Postfix, account creation will succeed, and Postfix will take care of delivering
  the message asynchronously.
* Postfix automatically retries delivering emails in case of transient errors, whereas Yoda and the EUS don't
  have this capability in the default configuration.
* Postfix has proper logging of email message delivery, whereas Yoda and the EUS have very limited email logging
  in the default configuration. Complete logging makes it possible to verify that emails have been sent, and
  to find the cause of any problems in case something has gone wrong.

In summary, using Postfix as a local MTA makes email delivery more resilient and helps with troubleshooting.

## Configuration

Configuration consists of three parts:
* Configure Postfix
* Configure the EUS to send emails to the Postfix instance
* Configure Yoda (ruleset) to send emails to the Postfix instance

### Postfix

In order to configure Postfix, you will need an account that can be used for sending email messages using
Secure SMTP. Example configuration:

```
enable_postfix: true

postfix_relayhost: smtp.myuniversity.nl
postfix_myhostname: yoda.myuniversity.nl
postfix_relayhost_username: smtpaccount@myuniversity.nl
postfix_relayhost_password: myPassword
```

### EUS

Configure the EUS to send email messages to the local Postfix instance:

```
eus_smtp_host: localhost
eus_smtp_port: 25
eus_smtp_auth: false
eus_smtp_security: false
```

### Yoda ruleset

Configure the Yoda ruleset to send email messages to the local Postfix instance:

```
smtp_server: smtp://localhost:25
smtp_auth: false
smtp_starttls: false
```

### See also

* [Troubleshooting email](troubleshooting-email.md)
# Keenable Community-forum
![cq_blog_perfect_collaboration_stack](https://github.com/Akshaykumar05/community-forum/assets/114390890/fa4260ac-5c68-46f1-98ea-03f868210f7c)

* I'm building an open-source Community platform for Keenable organization, similar to the Discord and Slack but this web application would be open-source so that we can configure it according to our need.
* So first we would see the local hosting and configarions of the web applicationand and later we would see it's deployment.
* For the web application I have selected **Zulip** an open-source tool and now step by step we'll go for the localhost. Let's go..

## Localhosting (Steps)
1. Download server
2. Install script
3. Create new organization
4. Connect SMTP service
5. Add Auth methods
6. Databases Service Configuration (PostgreSQL)
7. Video call integrations
8. Mobile push notifications
9. Maintenance
10. Connect

### Steps with details:
1. Download server

```
cd $(mktemp -d)
curl -fLO https://download.zulip.com/server/zulip-server-latest.tar.gz
tar -xf zulip-server-latest.tar.gz
```

* curl: Command-line tool for making HTTP requests.
* -f: Fail silently (i.e., don't show error messages).
* -L: Follow redirects (if the server responds with a redirect, curl will follow it).
* -O: Save the file with the same name as on the remote server.

2. Install script
* Now we’ll install it. This command uses Certbot for automatic SSL and assumes that you are root user.
```
./zulip-server-*/scripts/setup/install --certbot \
    --email=YOUR_EMAIL --hostname=YOUR_HOSTNAME
```
* So for zulip.gis.chat it becomes:
```
./zulip-server-*/scripts/setup/install --certbot \
    --email=my_support_email@my_domain.de --hostname=zulip.gis.chat
```
   
3. Create new organization
  ![Screenshot from 2023-12-29 18-31-37](https://github.com/Akshaykumar05/community-forum/assets/114390890/74371606-fe75-4714-8fd7-ff2e4cddf2a3)

4. Connect SMTP service
  * Outgoing email (SMTP) credentials that Zulip can use to send outgoing emails to users (e.g. email address confirmation emails during the signup process, message notification emails, password reset, etc.).
  * However, people cannot register yet, so let’s connect the SMTP service to Zulip. All you need to do is add the credentials from your SMTP provider.

* Enter them in /etc/zulip/settings.py and the password in /etc/zulip/zulip-secrets.conf.

```
EMAIL_HOST = "smtp-relay.sendinblue.com"
EMAIL_HOST_USER = "your_registration@email.com"
EMAIL_PORT = 587
```

 * Restart the server with command:

```
bash su zulip -c '/home/zulip/deployments/current/scripts/restart-server'
```
* **I got an error here during restarting the server, that was "outhentication denied".**
* **Another error was "403 Forbidden" during opening the link of locally hosted server.**
* **Error during re-installing the server.**
* **SMTP Error**
  * We tried to enable this service, but user is not getting the mail request. (23 Jan 24)
  * The most common error is not setting `ADD_TOKENS_TO_NOREPLY_ADDRESS=False` when
    using an email provider that doesn't support that feature.
```
connect: to ('Smtp.gmail.com', 587) None
reply: b'220 smtp.gmail.com ESMTP 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp\r\n'
reply: retcode (220); Msg: b'smtp.gmail.com ESMTP 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp'
connect: b'smtp.gmail.com ESMTP 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp'
send: 'ehlo vivek\r\n'
reply: b'250-smtp.gmail.com at your service, [2405:201:4019:618b:ecd1:7064:857d:deca]\r\n'
reply: b'250-SIZE 35882577\r\n'
reply: b'250-8BITMIME\r\n'
reply: b'250-STARTTLS\r\n'
reply: b'250-ENHANCEDSTATUSCODES\r\n'
reply: b'250-PIPELINING\r\n'
reply: b'250-CHUNKING\r\n'
reply: b'250 SMTPUTF8\r\n'
reply: retcode (250); Msg: b'smtp.gmail.com at your service, [2405:201:4019:618b:ecd1:7064:857d:deca]\nSIZE 35882577\n8BITMIME\nSTARTTLS\nENHANCEDSTATUSCODES\nPIPELINING\nCHUNKING\nSMTPUTF8'
send: 'STARTTLS\r\n'
reply: b'220 2.0.0 Ready to start TLS\r\n'
reply: retcode (220); Msg: b'2.0.0 Ready to start TLS'
send: 'ehlo vivek\r\n'
reply: b'250-smtp.gmail.com at your service, [2405:201:4019:618b:ecd1:7064:857d:deca]\r\n'
reply: b'250-SIZE 35882577\r\n'
reply: b'250-8BITMIME\r\n'
reply: b'250-AUTH LOGIN PLAIN XOAUTH2 PLAIN-CLIENTTOKEN OAUTHBEARER XOAUTH\r\n'
reply: b'250-ENHANCEDSTATUSCODES\r\n'
reply: b'250-PIPELINING\r\n'
reply: b'250-CHUNKING\r\n'
reply: b'250 SMTPUTF8\r\n'
reply: retcode (250); Msg: b'smtp.gmail.com at your service, [2405:201:4019:618b:ecd1:7064:857d:deca]\nSIZE 35882577\n8BITMIME\nAUTH LOGIN PLAIN XOAUTH2 PLAIN-CLIENTTOKEN OAUTHBEARER XOAUTH\nENHANCEDSTATUSCODES\nPIPELINING\nCHUNKING\nSMTPUTF8'
send: 'mail FROM:<vivekpandey10102002@gmail.com> size=630\r\n'
reply: b'530-5.7.0 Authentication Required. For more information, go to\r\n'
reply: b'530 5.7.0  https://support.google.com/mail/?p=WantAuthError 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp\r\n'
reply: retcode (530); Msg: b'5.7.0 Authentication Required. For more information, go to\n5.7.0  https://support.google.com/mail/?p=WantAuthError 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp'
send: 'rset\r\n'
reply: b'250 2.1.5 Flushed 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp\r\n'
reply: retcode (250); Msg: b'2.1.5 Flushed 12-20020a170902c24c00b001d770328a05sm3332281plg.36 - gsmtp'
```
    * After Resolving the error, I successfully sent the mail request from my side.

```
    root@moinuddin-G3-3579:/etc/zulip# su zulip -c '/home/zulip/deployments/current/manage.py send_test_email manishbarman117@gmail.com'
2024-01-25 04:55:45.828 ERR  [zulip.send_email] An SMTP username was set (EMAIL_HOST_USER), but password is unset (EMAIL_HOST_PASSWORD).  To disable SMTP authentication, set EMAIL_HOST_USER to an empty string.
If you run into any trouble, read:

  https://zulip.readthedocs.io/en/latest/production/email.html#troubleshooting

The most common error is not setting ADD_TOKENS_TO_NOREPLY_ADDRESS=False when
using an email provider that doesn't support that feature.

Sending 2 test emails from:
  * md.x.moinuddin@fosteringlinux.com
  * noreply@localhost

Successfully sent 2 emails to manishbarman117@gmail.com!
```
* But I face an issue that User unable to get mail request.

  * We got the suggestion on Allemp to install **Postfix** locally (same server) on the zulip system. So now I'm exploring Postfix.
    ### Postfix
    ![image](https://github.com/Akshaykumar05/community-forum/assets/114390890/fbbf42cf-7a5b-464b-bde4-f677b5c13ab9)

    * Postfix is a hugely-popular Mail Transfer Agent (MTA) designed to determine routes and send emails. This cross-platform server is open-source, free, and suitable for installation on the majority of UNIX-like operating systems.
    * [Suggested link](https://www.fosstechnix.com/how-to-configure-postfix-with-gmail-on-ubuntu/) (from allemp) to Configure Postfix with Gmail on Ubuntu
  
5. Add Auth methods
* Authentication Service facilitates username/password validation using your on-premises Active Directory/LDAP server. Authentication Service is installed as a virtual appliance and communicates with your local directory using LDAP over SSL. 
* I want to enable OAuth service with Zulip to login, that is: Google Authorization.
* Oauth is one of the most secure methods of API authentication, and supports both authentication and authorization.

* First of all we have uncommented the "Authentication_Backend" for Google OAuth service in "setting.py" and it can be seen below in atteched screenshots.

![Screenshot from 2024-01-30 17-00-56](https://github.com/Akshaykumar05/community-forum/assets/114390890/cc26692e-1039-452a-8051-b14d5bebdb32)

![Screenshot from 2024-01-30 17-02-08](https://github.com/Akshaykumar05/community-forum/assets/114390890/c9eeb32f-3d4f-420c-9677-d4760226ee31)

* Later we have to update the Google Cloud console according to the given documentation.
  
![Screenshot from 2024-02-01 13-12-35](https://github.com/Akshaykumar05/community-forum/assets/114390890/b0dca579-fd6d-49ab-9e2e-fee81d78f703)

* Let's go on the Google Cloud Console

![Screenshot from 2024-02-01 13-14-33](https://github.com/Akshaykumar05/community-forum/assets/114390890/1ea067aa-0e63-499a-869a-2586daf1a346)

![Screenshot from 2024-02-01 13-13-13](https://github.com/Akshaykumar05/community-forum/assets/114390890/b064ac76-04bb-4dae-942f-10564db8157a)

![Screenshot from 2024-02-01 13-12-50](https://github.com/Akshaykumar05/community-forum/assets/114390890/6e647b69-bc22-4496-a6af-139879745f9a)

![Screenshot from 2024-02-01 13-14-52](https://github.com/Akshaykumar05/community-forum/assets/114390890/8677305b-c3cb-4d49-b868-58c15c1763be)

![Screenshot from 2024-02-01 13-13-38](https://github.com/Akshaykumar05/community-forum/assets/114390890/128b1cc1-8e52-4a57-abe1-74d901eefce9)

![Screenshot from 2024-02-01 13-11-57](https://github.com/Akshaykumar05/community-forum/assets/114390890/eee49870-00b1-4d15-a9be-e2f21cfdfec0)






* Finally we have enabled the service, now the sign up page would be look like this
  
![google auth](https://github.com/Akshaykumar05/community-forum/assets/114390890/55a5bb88-b254-4bfc-80f7-6a96d6a75e93)

6. Databases Service Configuration (PostgreSQL)
* Now we have to do a configuration of PostgreSQL and fo this we'll follow the **"setting.py"**.
![Screenshot from 2024-02-08 00-08-36](https://github.com/Akshaykumar05/Keenable-Community-Forum/assets/114390890/2625794e-1dc7-4c77-a573-4a056043f88d)

* First we'll uncomment Postgres Services as mentioned below:
```
  REMOTE_POSTGRES_HOST = "dbserver.example.com"
  REMOTE_POSTGRES_PORT = "5432"
  REMOTE_POSTGRES_SSLMODE = "require"
```
* Now we have to restart the server with the command:
```
su zulip -c '/home/zulip/deployments/current/scripts/restart-server'
```

8. Video call integrations
   
9. Mobile push notifications
  * Push notifications are messages that can be sent directly to a user's mobile device. Unlike in-app messages, push notifications can appear on a lock screen or in the top section of a mobile device.

8. Maintenance
9. Connect
  

## Deployment Architecture
![Screenshot from 2023-12-29 00-16-05](https://github.com/Akshaykumar05/community-forum/assets/114390890/fb381617-f709-4a66-a32d-fa36bfc119eb)

### Deployment Components:
1. Nginx
2. Tornado
3. Django
4. PostgreSQL

#### 1. Nginx
![image](https://github.com/Akshaykumar05/community-forum/assets/114390890/7e09c553-d0d0-4434-8017-df51d47253e1)

* NGINX is open-source web server software used for reverse proxy, load balancing, and caching. It provides HTTPS server capabilities and is mainly designed for maximum performance and stability. It also functions as a proxy server for email communications protocols, such as IMAP, POP3, and SMTP.
  #### Features
  * Can handle 10,000 concurrent requests.
  * Catche HTTP requests
  * Act as Reverse Proxy
  * Act as a Load Balancer
  * Act as an API Gateway
  * Serve and Cache static files and images, videos, etc.
  * Handle SSL Certificate.
 
#### PostgreSQL
![image](https://github.com/Akshaykumar05/community-forum/assets/114390890/4236f2f5-8c11-47cb-b4ae-a8b48c22b3cf)

* It is a powerful, open-source Object-Rational Database Management System (ORDMS).
* It is used to store data securily.
* It is developed by the PostgreSQL Global Development Group (a team of volunteers), it is not controlled by any corporation or other private entity.

 #### History of PostgreSQL
 * It was started in 1986 by professor Stonebreaker as a follow up project. PostgreSQL is now the most advanced open-source database available anywhere.

 #### Features of PostgreSQL
 * It runs onall major OS like: Linux, Unix, and Windows.
 * It supports text, image, sound, video and include interfaces for many language like: C, C++, Java etc.
 * It supports a lot of features of SQL like: complex SQL queries, foreign key, triggers, views, transactions and concurrency etc.
 * In PostgreSQL, table can be set to inherit their characteristics from a "parent" table.
 * You can install several extentions to add additional funtionality to PostgreSQL.

## Deployment
* Deployment is the mechanism through which applications, modules, updates, and patches are delivered from developers to users.
* Deployment, also known as Software Deployment, is the process of installing, configuring, updating, and enabling one application or suite of applications that make a software system available for use, like facilitating a certain URL on a server.

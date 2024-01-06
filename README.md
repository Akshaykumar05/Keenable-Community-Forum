# Community-forum
* I'm building an open-source communication platform for Keenable organization, similar to Discord and Slack but it will be self hosted.
* For that I have selected **Zulip** tool.

## Self-hosting (Steps)
1. Download server
2. Install script
3. Create new organization
4. Connect SMTP service
5. Add Auth methods
6. Mobile push notifications
7. Maintenance
8. Connect

### Steps with details:
1. Download server
  
  ![Screenshot from 2024-01-01 14-11-12](https://github.com/Akshaykumar05/community-forum/assets/114390890/93d990b6-4e25-4a0d-afd1-5c8e397f1678)

2. Install script
   
4. Create new organization
  ![Screenshot from 2023-12-29 18-31-37](https://github.com/Akshaykumar05/community-forum/assets/114390890/74371606-fe75-4714-8fd7-ff2e4cddf2a3)

5. Connect SMTP service
  * Outgoing email (SMTP) credentials that Zulip can use to send outgoing emails to users (e.g. email address confirmation emails during the signup process, message notification emails, password reset, etc.).
  * However, people cannot register yet, so let’s connect the SMTP service to Zulip. All you need to do is add the credentials from your SMTP provider.

* Enter them in /etc/zulip/settings.py and the password in /etc/zulip/zulip-secrets.conf.

''' EMAIL_HOST = "smtp-relay.sendinblue.com"
EMAIL_HOST_USER = "your_registration@email.com"
EMAIL_PORT = 587 '''

 * Restart the server with command:

''' su zulip -c '/home/zulip/deployments/current/scripts/restart-server' '''

5. Add Auth methods
  * Authentication Service facilitates username/password validation using your on-premises Active Directory/LDAP server. Authentication Service is installed as a virtual appliance and communicates with your local directory using LDAP over SSL.
    
6. Mobile push notifications
  * Push notifications are messages that can be sent directly to a user's mobile device. Unlike in-app messages, push notifications can appear on a lock screen or in the top section of a mobile device.

8. Maintenance
9. Connect
  

## Deployment Architecture
![Screenshot from 2023-12-29 00-16-05](https://github.com/Akshaykumar05/community-forum/assets/114390890/fb381617-f709-4a66-a32d-fa36bfc119eb)

### Deployment
1. Djongo

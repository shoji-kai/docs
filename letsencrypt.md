# letsencrypt

## インストール

```
% sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): sho2kai+hyakusho@gmail.com
Starting new HTTPS connection (1): acme-v01.api.letsencrypt.org

-------------------------------------------------------------------------------
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v01.api.letsencrypt.org/directory
-------------------------------------------------------------------------------
(A)gree/(C)ancel: A

-------------------------------------------------------------------------------
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about EFF and
our work to encrypt the web, protect its users and defend digital rights.
-------------------------------------------------------------------------------
(Y)es/(N)o: N

Which names would you like to activate HTTPS for?
-------------------------------------------------------------------------------
1: blog.shojikai.com
-------------------------------------------------------------------------------
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1
Obtaining a new certificate
Performing the following challenges:
tls-sni-01 challenge for blog.shojikai.com
Waiting for verification...
Cleaning up challenges
Deployed Certificate to VirtualHost /etc/nginx/sites-enabled/default for set(['blog.shojikai.com'])

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): __2
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/default

-------------------------------------------------------------------------------
Congratulations! You have successfully enabled https://blog.shojikai.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=blog.shojikai.com
-------------------------------------------------------------------------------

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/blog.shojikai.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/blog.shojikai.com/privkey.pem
   Your cert will expire on 2018-04-09. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

## ドメインの追加

```
% sudo certbot certonly --cert-name blog.shojikai.com -d shojikai.com,blog.shojikai.com,mail.shojikai.com
Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------------
1: Nginx Web Server plugin - Alpha (nginx)
2: Spin up a temporary webserver (standalone)
3: Place files in webroot directory (webroot)
-------------------------------------------------------------------------------
Select the appropriate number [1-3] then [enter] (press 'c' to cancel): 2
Plugins selected: Authenticator standalone, Installer None
Starting new HTTPS connection (1): acme-v01.api.letsencrypt.org

-------------------------------------------------------------------------------
You are updating certificate blog.shojikai.com to include domains: shojikai.com,
blog.shojikai.com, mail.shojikai.com

It previously included domains: blog.shojikai.com

Did you intend to make this change?
-------------------------------------------------------------------------------
(U)pdate cert/(C)ancel: U
Renewing an existing certificate
Performing the following challenges:
tls-sni-01 challenge for shojikai.com
tls-sni-01 challenge for blog.shojikai.com
tls-sni-01 challenge for mail.shojikai.com
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/blog.shojikai.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/blog.shojikai.com/privkey.pem
   Your cert will expire on 2018-04-09. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

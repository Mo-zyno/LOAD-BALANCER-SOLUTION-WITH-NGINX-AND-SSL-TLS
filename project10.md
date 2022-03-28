
# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

**In this project we will configure an Nginx Load Balancer solution.**

**It is also extremely important to ensure that connections to your Web solutions are secure and information is encrypted in transit – we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.**

**When data is moving between a client (browser) and a Web Server over the Internet – it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-In-The-Middle (MIMT) attack.**

**This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by cybercriminals.**

**SSL and its newer version, TSL – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used.**

**SSL/TLS uses digital certificates to identify and validate a Website. A browser reads the certificate issued by a Certificate Authority (CA) to make sure that the website is registered in the CA so it can be trusted to establish a secured connection.**

## Task

*This project consists of two parts:*

1. Configure Nginx as a Load Balancer

2. Register a new domain name and configure secured connection using SSL/TLS certificates

**Your target architecture will look like this:**

![alt text](./images/nginx%20image.png)

## LETS GET STARTED

### STEP 1. CONFIGURE NGINX AS A LOAD BALANCER

1. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)

![alt text](./images/Nginx%20instance%20created.png)

![alt text](./images/HTTP%2080%20and%20HTTPS%20443%20open%20.png)

2. Register a Free Domain Name (DNS) for the NGINX Server.

![alt text](./images/register%20domain.png)

![alt text](./images/Domain%20name%20registered.png)

*create a hosted zone in Route53*

![alt text](./images/type%20dmain%20name%20and%20create%20hosted%20zone.png)

*Copy the following route traffic* 

![alt text](./images/value%20route%20traffic.png)

![alt text](./images/name%20server.png)

*paste each in the domain name manage servers*

![alt text](./images/manage%20servers.png)

![alt text](./images/successfully%20done.png)

*Create a record for the Domain name*

![alt text](./images/create%20record.png)

*Input the Nginx Private IP addrr*

![alt text](./images/Nginx%20IP%20address.png)

*Create a second Record with www for the Domain*

![alt text](./images/second%20record%20www.png)

![alt text](./images/two%20records%20created.png)


### UPDATE SERVER AND INSTALL NGINX

`sudo apt update`

![alt text](./images/sudo%20apt%20update%20nginx.png)

`sudo apt install nginx -y`

![alt text](./images/install%20Nginx.png)

1. Enabled nginx and started it using the following cmdlet;

2. Checked to see if nginx has been successfully installed using the following cmdlet;

`sudo systemctl enable nginx`

`sudo systemctl start nginx`

`sudo systemctl status nginx`

![alt text](./images/sudo%20enable%20start%20and%20status%20nginx.png)


3. Created a configuration for the reverse proxy settings by copying the web server configuration into the load balancer using the followng cmdlet;

*Created a config file in the NGINX sites-available folder as follows:*

`sudo nano /etc/ngingx/sites-available/load_balancer.conf`

![alt text](./images/nano%20config%20file.png)

4. Removed the default site to allow the reverse proxy to redirect to the new configuration file using the cmdlet below:

`sudo rm -f /etc/nginx/sites-enabled/default`

5. Checked if nginx is successfully configured using the following cmdlet:

`sudo nginx -t`

6. Navigated to the nginx sites-enabled folder using the cmdlet below to link the config file created in the     sites-available with the sites-enabled:

`cd /etc/nginx/sites-enabled/`

7. Linked the Load Balancer config file created in the site-available to the site enabled so that the nginx can access the configuration to it using the cmdlet below:

`sudo ln -s ../sites-available/load_balancer.conf`

8. Reloaded nginx using the following cmdlet below:.

`sudo systemctl reload nginx`

![alt text](./images/nano%20config%20for%20nginx.png)


9. Checked the 'http://toolingeon.ml' domain/site and saw that it redirects to the tooling page.
   But our site is unsecured so lets secure it with SSL/TLS CERTIFICATES in STEP 2.

![alt text](./images/unsecured%20site%20from%20nginx%20server.png)


### STEP 2. CONFIGURED SECURED CONNECTION USING SSL/TLS CERTIFICATES AS FOLLOWS:

1. Ran the following cmdlet below to install the certbot software.

`sudo apt install certbot -y`

![alt text](./images/install%20certbot.png)

2. Installed a python dependency module to be used by the certbot using the following cmdlet: 

`sudo apt install python3-certbot-nginx -y`

![alt text](./images/python%203%20certbot.png)

3. Checked nginx syntax again and reloaded after the installation of certbot and python3 dependency.

`sudo nginx -t && sudo nginx -s`

![alt text](./images/syntax%20and%20config%20succesful.png)

4. Created a certificate for our domain to secure it by running the following cmdlet:

`sudo certbot --nginx -d toolingeon.ml -d`

![alt text](./images/certbot%20new%20certficate.png)

5. Refreshed the 'toolingeon.ml' web page to observe the applied security settings after creating certificate

![alt text](./images/site%20secured.png)

6. As best practice, created a cronjob that will run a renew command periodically to automatically renew our certificate by editing the crontab

`crontab -e `

![alt text](./images/crontab%20-e.png)

script to the file, choose #1 for Nano editor: * */12 * * * root /usr/bin/certbot renew > /dev/null 2>&1

![alt text](./images/crontab%20-e.png)

## We have created an NGINX load balancer and can route traffic to it via reverse proxy and also secured it.
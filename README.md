# Install-Nginx-and-Configure-SSL

# Install Nginx on Host

→ sudo apt-get install nginx

Change the configuration at /etc/nginx/conf.d/default

Note :- replace your site name where you see www.dominname.in

server 
{
        server_name www.dominname.in;
	
	location /.well-known/ {
	
	root /var/www/www.domainname.in/.well-known/;
	
        }
	
	location / {
	
        return 301 https://$server_name$request_uri;
	
	}
}


This configuration does two things 

Prepares a static folder for LetsEncrypt to use later on port 80

Sets up a redirect to the HTTPS-enabled version of your site for any calls on port 80

Obtain an HTTPS certificate from LetsEncrypt

Before we enable Nginx we'll need to obtain a certificate for your domain. HTTPS encrypts the HTTP connection between your users and your site. It is essential when you use the admin page.
Use certbot to get a certificate.

→ sudo apt-get install software-properties-common

→ sudo apt update

→ sudo apt-get install python3-certbot-nginx 

→ sudo certbot --authenticator webroot --installer nginx

It will ask for the webroot path 

→ /var/www/html/

After getting the SSL certificate change the config for HTTPS

Edit /etc/nginx/conf.d/default:

Note :- replace your site name where you see sanela.in

server {
	server_name sanela.in;
	listen 443 ssl;

	location / {
		Proxy_pass http://DESTINATION:PORT(127.0.0.1:8080);
	        proxy_set_header    X-Real-IP $remote_addr;
	        proxy_set_header    Host      $http_host;
		proxy_set_header X-Forwarded-Proto https;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

	}

	ssl_certificate     /etc/letsencrypt/live/www.domainname.in/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/www.domainname.in/privkey.pem;
	ssl on;
 
}


# Enable Nginx

You can now start and enable Nginx, then head over to your URL in a web browser to test that everything worked.

→  sudo systemctl enable nginx

→  sudo systemctl start nginx

# To Renew SSL certificate:

→ sudo certbot -v

Select the domain name in the list and select redirect.

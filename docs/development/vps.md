# VPS Settings

The Bot is hosted on a fre EC2 instance on AWS running Ubuntu 20.04.4 LTS so a bit of linux knowledge is required.
All Ports on the server are closed except 22 for ssh traffic and 443 for https.
The server also has an elastic ip so it is always reachable.

## Nginx Config

On the server runs nginx configured as a reverse proxy, which lets us establish an https connection, so that the traffic is always encrypted.
The configuration file can be found in "/etc/nginx/sites-enabled/default" and looks like this:

```text
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##
#
server {
   listen 443 ssl;
   server_name preview.webhook.metisbot.xyz;
   ssl_certificate  /etc/letsencrypt/live/preview.webhook.metisbot.xyz/fullchain.pem;
   ssl_certificate_key  /etc/letsencrypt/live/preview.webhook.metisbot.xyz/privkey.pem;
   ssl_prefer_server_ciphers on;
   location /form {
        proxy_pass http://localhost:3000/form;
        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;
 }
   location /git {
        proxy_pass http://localhost:3000/git;
        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;
        }
   location /ws {
        proxy_pass http://localhost:3000/;
        proxy_set_header        Upgrade $http_upgrade;
        proxy_set_header        Connection "Upgrade";
        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;
        }
}
```

For more help please consult [this great Guide by Microsoft](https://docs.microsoft.com/en-us/troubleshoot/developer/webapps/aspnetcore/practice-troubleshoot-linux/2-2-install-nginx-configure-it-reverse-proxy)

## SSL Certificate

We used Certbot to create the ssl certificate. To create the certificate please follow the instructions on [here](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal).
In short:

- install cerbot with snap
- make sure port 80 is open in aws
- run `cerbot certonly --nginx`

This will create two certificates. Afterwards tell nginx in its config where it can find the certificates:

```bash
ssl_certificate  /etc/letsencrypt/live/webhook.metisbot.xyz/fullchain.pem;
ssl_certificate_key  /etc/letsencrypt/live/webhook.metisbot.xyz/privkey.pem; 
```

__Important!__ Don't forget to open port 443 in aws, otherwise your server won't be reachable

**FAQ:**

Q: Server is unreachable even if I configured all the ports correctly
A: See https://stackoverflow.com/a/54810101


## Custom Domain

Because we are using a custom domain we created an A record that points webhook.metis.xyz to our server IP.
Custom domains can be bought cheaply on sites like [namecheap](https://namecheap.com) or [godaddy](https://godaddy.com)

---
layout: post
title:  "Manually renewing NGINX certbot wildcard certs"
date:   2019-10-20 00:57:51 -0400
categories: nginx ssl letsencrypt
---

This is an SOP for updating lets-encrypt wildcard certificates that have expired on Nginx.

## 1. Pre-requisites
If the server / instance hasn't died since the last time you've done this, you can skip this step.

### 1.1 Update Repositories
This is an optional step, however, chances are it likely hasn't been done since the last cert update.
```
sudo apt-get update && sudo apt-get upgrade
```

### 1.2 Install certbot
These instructions assume you're running Ubuntu 18.04 LTS, if this isnt the case see the most up to date instructions at <https://certbot.eff.org/>.
#### 1.2.1 Add the certbot PPA
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
```

#### 1.2.2 Install certbot
```
sudo apt-get install certbot python-certbot-apache
```

## 2. Generating the certificates
### 2.1 Generate the certificates
Using certbot, generate new certificates.
```
certbot-auto certonly \
--manual \
--prefered-challenges=dns \
--email=<your_email@email.com> \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.domain.tld
```

After executing the above command, the Certbot will share a text record to add to your DNS.
```
Please deploy a DNS TXT record under the name
_acme-challenge.domain.tld with the following value:
J50GNXkhGmKCfn-0LQJcknVGtPEAQ_U_WajcLXgqWqo
```

Follow the instructions & create the TXT records in your registrar console until the certificates are generated. Once the certificates are generated, certbot will output the location of the generate certificates - __make a note of this location__!

### 2.2 Update NGINX certificates
At this point nginx still doesn't have the latest certs, the easiest way to do this is to use the certbot nginx plugin.

```
sudo certbot certonly --nginx
```

Certbot will then prompt you to replace the certs in nginx with the ones generated in the above step, follow the prompts and then restart NGINX.

```
sudo nginx -t
sudo systemctl restart nginx
```

The final step is to verify sites are displaying valid certs and you're done!

## Credits
* <https://medium.com/@saurabh6790/generate-wildcard-ssl-certificate-using-lets-encrypt-certbot-273e432794d7>
* <https://certbot.eff.org/lets-encrypt/ubuntubionic-apache.html>
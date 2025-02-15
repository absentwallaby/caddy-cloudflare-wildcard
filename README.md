# caddy proxmox setup

use script from communityscripts to create lxc

then inside lxc console

1. (optional) setup ssh using certificate

```
mkdir .ssh
cd .ssh
nano authorized_keys
# paste key, then save & exit
chmod 600 authorized_keys
cd
```

2. build dns cloudflare module into caddy:

```
xcaddy build --with github.com/caddy-dns/cloudflare
mv /usr/caddy /usr/bin/caddy.backup
mv ~/caddy /usr/bin/caddy

sudo systemctl edit caddy

# in top area of file between comments, add

[Service]
Environment="CF_API_TOKEN=cloudflare-token-goes-here"
Environment="TOP_DOMAIN=owned-domain-name"
```

3. if you haven't already - add the dns names used in the caddy file to your dns server. (out of scope of this document)

4. load the caddy file (can only be done from the caddy server itself)
- note - you will need to get the caddyfile on to the machine for this.

```
curl localhost:2019/load  -H "Content-Type: text/caddyfile"       --data-binary @Caddyfile
```

5. browse to one the domain name in your browser - you should get a browser certificate error.. if you then have a look at the certificate that was used - you can see it was from the "(STAGING) Let's Encrypt" organisation. This indicates success

6. if that is successful - then comment out the acme_ca line in the caddyfile, reload the caddy file - and then you should get valid certificates. 


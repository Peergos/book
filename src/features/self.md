# Self hosting

Peergos is fully self hostable. You can run peergos from your own home or server to obtain as much storage and bandwidth as you need, whilst still transparently interacting with anyone using any other server. Because the server only ever sees encrypted data you can also tell it to directly store your data in a standard cloud storage provider like Backblaze or Amazon without any loss of privacy.

To self host, we recommend at least 2 CPU cores and 2 GiB of RAM. You can use a local file backed blockstore, or an S3 compatible service. You can use local sqlite db, or postgres compatible. We recommend sticking with sqlite, even if you plan on having 1000s of users. The database is very small (~10MB per 1000 users).

<img alt="Self host at home or on your own server" src="/img/self-host.svg" class="center" style="width: 100%;" />

## Getting started

1. Download a release from [https://peergos.net/public/peergos/releases](https://peergos.net/public/peergos/releases)
2. Install Java >= 25 (only required for the jar; native packages bundle their own runtime)
3. Run Peergos with:

> java -jar Peergos.jar daemon -generate-token true

All Peergos data is stored in `~/.peergos` by default. Override this with the `PEERGOS_PATH` environment variable or arg.

The config is written to `$PEERGOS_PATH/config` on first run, so subsequent runs only need:

> java -jar Peergos.jar daemon

It needs a publicly routeable IP address, and inbound tcp and udp on port 4001 should be allowed. 

Browse to the localhost address printed on startup (it includes a single use signup token) to create your account.

## Public IP

Some cloud hosts don't add your public IP to the network interfaces by default. If yours doesn't, add it manually, for example:

> sudo ip address add MY.PUBLIC.IP dev eth0

Peergos works behind NATs and firewalls, but a server with a publicly routable IP and an always-on machine is best for a home server.

## Domain name and TLS

You can log in to your self-hosted account from another Peergos instance (e.g. one running on your laptop) without setting up a domain or TLS - connections are routed securely over P2P TLS 1.3 streams. Writes are proxied to your home server so data is always persisted there.

If you want to expose the web interface publicly, set up a domain and TLS certificate (we recommend nginx and letsencrypt) and add:

> -public-server true

If a reverse proxy like nginx is terminating TLS, also tell Peergos your domain:

> -public-domain $YOUR_DOMAIN

The TLS certificate must cover the wildcard subdomain so that apps (PDF viewer, text editor, calendar, third party apps) work. Set A records for both `$YOUR_DOMAIN` and `*.$YOUR_DOMAIN`.

### Example nginx config

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    location ^~ /.well-known {
         allow all;
         proxy_pass http://127.0.0.1:8888;
    }

    return 301 https://$host$request_uri;
}

server {
        listen 443 ssl http2;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_session_timeout 10m;

        ssl_ciphers TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:!TLS_AES_128_GCM_SHA256;
        ssl_certificate /etc/letsencrypt/live/$YOUR_DOMAIN_NAME/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/$YOUR_DOMAIN_NAME/privkey.pem;

        add_header Strict-Transport-Security "max-age=31536000" always;
        server_name $YOUR_DOMAIN_NAME;

        client_max_body_size 2M;

        location / {
                proxy_pass http://127.0.0.1:8000;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $http_host;
                allow all;
        }

        location ^~ /.well-known {
                 allow all;
                 proxy_pass http://127.0.0.1:8888;
        }
}
```

On SELinux enabled distributions, allow nginx to access port 8000 (`tcp_socket`).

## Docker

```
docker pull ghcr.io/peergos/web-ui:master

docker run --volume $(PEERGOS_PATH):/opt/peergos/data ghcr.io/peergos/web-ui:master \
    daemon -listen-host 0.0.0.0 \
    -public-domain $YOUR_DOMAIN_NAME -public-server true \
    -announce-ipfs-addresses /ip4/$IP/tcp/4001,/ip4/$IP/udp/4001/quic-v1 \
    -log-to-console true
```

## S3 compatible blockstore

Add the following to the command line (or the `.peergos/config` file after the first run):

```
-use-s3 true
-authed-s3-reads true
-direct-s3-writes true
-s3.accessKey $ACCESS_KEY
-s3.bucket $BUCKET
-s3.region $REGION
-s3.region.endpoint $ENDPOINT
-s3.secretKey $SECRET_KEY
```

For Minio set `-authed-s3-reads false -direct-s3-writes false`.

The bucket needs the following CORS policy:

```
<CORSConfiguration>
  <CORSRule>
    <AllowedOrigin>https://$YOUR_DOMAIN</AllowedOrigin>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
    <ExposeHeader>ETag</ExposeHeader>
    <ExposeHeader>Content-Length</ExposeHeader>
    <MaxAgeSeconds>3600</MaxAgeSeconds>
  </CORSRule>
</CORSConfiguration>
```

## Postgres instead of sqlite

```
-use-postgres true
-postgres.database $DATABASE
-postgres.host $HOST
-postgres.password $PASSWORD
-postgres.username $USERNAME
```

## Troubleshooting

* Add `-log-to-console true` to see logging on the console as well as in the log file.
* The very first run syncs the PKI and takes several minutes. Subsequent runs start within seconds.
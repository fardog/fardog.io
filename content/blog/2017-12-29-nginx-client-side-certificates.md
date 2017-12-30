---
title: Client-Side Certificate Authentication with nginx
date: 2017-12-29T22:10:00-07:00
categories: [nginx, security]
---

Authentication in applications is tough. If you decide to roll your own,
security issues are nearly guaranteed. Most anyone who writes software for a
living will tell you to use something you _didn't_ write; that's battle-tested
and in wide use. Open source is even better; hopefully that many eyes and that
many users will suss out the bugs.

Still, there _will be bugs_. Which is what I think every time I see a
login page on some device in my home. Who wrote this? Is it even remotely
secure? Definitely I don't want to expose this to the internet.

So: let's put something in front of it that can handle authentication for us!
In this case, it's a system which is unauthenticated, but that we want to expose
with auth that says: this person's allowed. The upstream application doesn't
care who they are, just that they're allowed in.

In fact: I don't even want to deal with passwords. I'd prefer to authenticate
devices instead.

These are some notes on configuring client-side certificate authentication with
[nginx][], which reverse proxies to an application server.

_I don't expect that I've got everything right here;_ please [file an issue][i]
against this repository if you see anything troubling here. Consider this my
notes so that I remember how to do things again.

# What is a Client-Side Certificate?

A client-side certificate is a transport-layer authentication mechanism; it can
be used to verify a user _before_ the application layer. In terms of a web app,
it happens at the "S" of "HTTPS": the client is authenticated when the TLS
handshake occurrs, and _not_ at the HTTP layer that is tunneled over the secure
connection.

The [Wikipedia article on TLS][tls] explains it better than I ever could. But
the important thing to understand is that when done right, your application can
assume that the information it's receiving is coming from someone who's already
authenticated. In my case, the application doesn't care about that: we're just
using it to ensure only devices I allow may access the application.

For that reason, this article only covers the "device may access application"
portion; using this method to do full authentication may be covered in a later
article.

# Creating the Certificate Authority

First, you must create a key for your Certificate Authority (CA); this key will
be used to create the server-side certificate, and will sign all client
certificate requests. That's to say: it's the master "password" for the whole
system. Generate one, and keep it safe.

```bash
openssl genrsa -des3 -out ca.key 4096
```

You'll be asked to encrypt the key with a passphrase. Be sure to note it, as
you'll be asked for it every time you create a new certificate or sign a
client certificate request.

## Create a CA Certificate

Next create a CA Certificate; this is the server-side certificate that will be
sent via the TLS server to the client. Note this certificate is specific to the
client-side certs, and is not a replacement for your typical certificate needed
for HTTPS authentication; we'll get to that later.

```bash
# sign a certificate for 365 days; replace that number with whatever's
# suitable for your application
openssl req -new -x509 -days 365 -key ca.key -out ca.crt
```

You'll be asked a number of questions; here's what I've found:

* Note what you've entered for Country, State, Locality, and Organization;
  you'll want these to match later when you renew the certificate.
* Do not enter a common name (CN) for the certificate; I'm unsure why, but I
  had problems when I entered one.
* Email can be omitted.

Renewing a certificate just requires running the same command; to generate a
new certificate. If you need to see what you entered in the old certificate, you
can run:

```bash
openssl x509 -in ca.crt -noout -text
```

This will list all information about the certificate, including the values
mentioned above.

# Creating a Client Certificate

Next, a client certificate will be created; it's up to you how you want to
handle these, but remember this is effectively a password, and if you want to
"change" it, you must revoke the certificate. So, think about how you'd like to
handle your authentication: per user, per device?

You'll want a client key first; this is generated in the same manner as the
certificate key; you could even use that one (but don't). Typically, if you had
a number of users, the next two steps would be the ones you'd ask them to do,
to create a certificate signing requests for you do sign.

## Users: Create a Key, and a Certificate Signing Request

Create an RSA key, if you don't have one already:

```bash
openssl genrsa -des3 -out user.key 4096
```

Then, create a Certificate Signing Request (CSR)

```bash
openssl req -new -key user.key -out user.csr
```

A number of questions will be asked; answer each one, including the
Common Name (CN) and email address. The CSR that's created would be sent to the
CA (an administrator, but in this case probably also yourself) to be signed.

# Signing a CSR

A CSR must now be signed by the CA; this is the CA saying "I know this person
or device: they are who they say they are."

```bash
# sign the csr to a certificate valid for 365 days
openssl x509 -req -days 365 -in user.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out user.crt
```

You'll typically want to increment the serial number with each signing. Once the
certificate expires, a new CSR doesn't need to be recreated; the same one can
be signed, which will create a new certificate tied to that public key.

The signed certificate would be sent back to the user along with the CA cert
(not private key!), for installation on their device.

# Creating a PKCS #12 (PFX)

Now the signed certifcate must be made installable on a device in a way that
bundles the client keys and certificate. The resultant archive is effectively
a password, so it must be kept as safe as the other private keys.

To create the pfx:

```bash
openssl pkcs12 -export -out user.pfx -inkey user.key -in user.crt -certfile ca.crt
```

You will be asked to supply an "export password", and it's very recommended that
one is set, since often you'll need to transfer the PFX archive to a device such
as your phone; you don't want this sitting in your email without a password on
it.

The PFX archive can now be imported into your web browser. This is pretty neat!
You can see how a process like this can be used to prove identity and create
valid access for a user, without either the administrator (CA) or the user
revealing their private key to one another.

Now, lets look at setting up nginx for certificate auth, with a reverse proxy
to our unauthenticated application.

# nginx Setup

A minimal `nginx.conf` that supports certificate auth, http redirected to https
and a reverse proxy would look as follows for a domain `example.com`. Note that
the HTTPS certificate in this example is provided by [letsencrypt][]. That's not
covered here, but may be in some future post.

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
  worker_connections 768;
}

http {
  # some HTTP boilerplate
  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  server_tokens off;

  include /etc/nginx/mime.types;
  default_type application/octet-stream;

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;

  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;

  gzip on;
  gzip_disable "msie6";

  map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
  }

  # server on port 80 for HTTP -> HTTPS redirect
  server {
    listen 80;
    server_name example.com;
    return 301 https://example.com$request_uri;
  }

  # The letsencrypt-secured HTTPS server, which proxies our requests
  server {
    listen 443 ssl;
    server_name example.com;

    ssl_protocols TLSv1.1 TLSv1.2;
    # letsencrypt certificate
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # client certificate
    ssl_client_certificate /etc/nginx/client_certs/ca.crt;
    # make verification optional, so we can display a 403 message to those
    # who fail authentication
    ssl_verify_client optional;

    access_log /var/log/nginx/example.com;

    location / {
      # if the client-side certificate failed to authenticate, show a 403
      # message to the client
      if ($ssl_client_verify != SUCCESS) {
        return 403;
      }

      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;

      # Fix the "It appears that your reverse proxy set up is broken" error.
      proxy_pass          http://localhost:8080;
      proxy_read_timeout  90;

      # web sockets
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;

      proxy_redirect      http://localhost:8080 https://example.com;
    }
  }
}
```

Of course, in this example the server running on `localhost:8080` would _not_
be exposed to the world; either it'd be running on a protected box inside a
local network, or firewalled off from the outside.

Now, when you visit the nginx server, your browser will be prompted for its
client certificate; select the certificate that you installed, and you should
be proxied through to the upstream server. If you visit from a browser without
client certificates installed, you should see a 403 without any sort of prompt.

# Misc

A few additional thoughts:

I documented this process because it's easily forgettable, and a hassle to
re-figure when you forget and it's time to renew your certs. A thing I'd like
to spend more time on in the future is a tool to make this process easier for
folks.

This document assumes a lot, and also assumes that you know _something_ before
coming in; don't assume that following this document will give you a secure
setup! I might not even be correct in what I'm doing!

Have fun, stay safe.

[nginx]: https://nginx.org/
[i]: https://github.com/fardog/fardog.io/issues
[tls]: https://en.wikipedia.org/wiki/Transport_Layer_Security#Client-authenticated_TLS_handshake
[letsencrypt]: https://letsencrypt.org/

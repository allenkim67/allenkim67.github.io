---
layout: post
title: Adding SSL To Express For Free
categories: programming
tags: [ssl, node, express]
---

Here, I'll show you how to setup free SSL verification with [Let's Encrypt](https://letsencrypt.org/).

### Set up the server

```
const fs = require('fs');
const https = require('https');
const express = require('express');

// we'll generate these files in the next step
const options = {
  key: fs.readFileSync('/etc/letsencrypt/live/example.com/privkey.pem'),
  cert: fs.readFileSync('/etc/letsencrypt/live/example.com/fullchain.pem')
};

const app = express();

const server = https
  .createServer(options, app)
  .listen(443, () => console.log("listening on port 443"));
```

Note that you are using the `https` module, not `http`.

The main difference is that `https.createServer` takes the paths to your
certificates as its first argument.

Note https uses port 443 by convention instead of port 80 for http.

Finally, since the current code only handles https requests you'll probably want
something to handle http requests as well. [Here is an example that redirects all http requests to https.](http://stackoverflow.com/questions/24015292/express-4-x-redirect-http-to-https)

### Generating the certificates
First you want to download [certbot](https://certbot.eff.org/) which is [recommended by Let's Encrypt](https://letsencrypt.org/docs/client-options/).
If you're on mac you can just `brew install certbot`. Otherwise go to the site
and follow the directions.

Then run this in the terminal replacing your email and site domain:

`certbot certonly --standalone --email admin@example.com -d example.com`

That should create your certificates at `/etc/letsencrypt/live/example.com/` by
default. If you want to tweak the certbot options you can check out their [documenation](https://certbot.eff.org/docs/using.html).
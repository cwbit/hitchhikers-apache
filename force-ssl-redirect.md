## Forcing an SSL Redirect with Apache2

The idea here is simple, you need two virtual hosts

* the non-secure (`http`) version (ie. port `80`)
* the secure (`https`) version (ie. port `443`)

All we're going to do is edit the virtual hosts config file for the `HTTP` version and tell it to immediately and permanently redirect to the `HTTPS` virtual host

```ini
<VirtualHost *:80>
  ServerName foo.example.com
  Redirect permanent / https://foo.example.com/
</VirtualHost>

# .. comment out anything else I guess, it won't be used .. #
```

> NOTE: It's important to keep the trailing `/` on the `https://../` otherwise the URL will get re-built without it (and everything will break)

There is a way to do this with `.htaccess` files but it's not recommended and is probably slower as the `VirtualHost` is much earlier in the processing chain and would redirect earlier.

### if you need to redirect just a certain subset

If you need to redirect just a certain subset of the app, you can do

```ini
Redirect permanent /login https://example.com/login
```

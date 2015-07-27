# .htaccess

* [Caching assets](#caching-assets)
* [Disable ETags](#disable-etags)

## Caching Assets

Caching your assets can greatly improve page-load times and is highly recommended by Google PageInsights and Yahoo!'s YSlow, which are the basis for many online speed-evaluation algorithims, and are probably the algorithm(s) actually determining your online ranking.

In a nutshell, you can add headers to the response objects containing things like images, css, js, etc to let the client browser know how long it can keep it's copy of the asset. 

If, for example, your images rarely ever change there's no need for the client to re-download them constantly. Just cache it the first time, and use the cache until it expires, or until the asset name changes (more on that below).

The following snippet is modified/inspired by the version found [here](http://fortheloveofseo.com/blog/performance/leverage-browser-caching-how-to-add-expires-headers/)

```apache
<IfModule mod_expires.c>
    # Enable expirations
    ExpiresActive On
    
    # Turn off ETags (esp if on load-balanced servers)
    FileETag None

    # Default directive
    ExpiresDefault "access plus 1 month"

    # Images
    ExpiresByType image/* "access plus 1 month"

    # favicon image override (if needed)
    # ExpiresByType image/x-icon "access plus 1 year"

    # CSS
    ExpiresByType text/css "access plus 1 month"

    # Javascript
    ExpiresByType application/javascript "access plus 1 month"
</IfModule>

```

The code above should be pasted into your `.htaccess` file in your webroot.

Note the long file cache times. The strategy we find works best in all cases is to ***CHANGE THE ASSET FILENAME*** when you want to invalidate the cached object.

* Any time an asset has the same name, it should be safe to use the existing copy.
* If the asset has a new name, it should be safe to assume something has been changed, and your browser will automatically re-download and re-cache the new object.

An easy way to get unique names is to add a timestamp or hash to the filename.

* `site-theme.css` becomes `site-theme.a49bde3c.css`
* `some-image.jpg` becomes `some-image.20150101.jpg`

Any time the browser sees a change in the filename it will automatically download and re-cache the asset, giving you the best of both worlds; *Caching that can be reset on-demand*.

## Disable ETags

***WHAT?*** ETags are used for no-setup asset caching. It is generally considered to be a best practice to turn them off and implement a more specific, controllable method for asset caching [like the one above](#caching-assets)

***WHY?*** ETags are a cookie-cutter way to implement browser-level asset caching, but suffer a few inherit setbacks

* They are tied to the machine the asset lives on - causing issues with load-balanced servers (default, configurable)
* Leaves cache decision up to hosting server(s)

***HOW?*** First, make sure you've implemented an alternative caching strategy. It'll do you no good to remove your only caching option and not have a valid replacement! If you're looking for a good alternative, see our section on [how to properly cache your assets](#caching-assets).

Second, add the following line to your `.htaccess` file in your webroot (if it doesn't already exist)

```apache
FileETag None
```
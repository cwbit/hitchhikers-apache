#How to install Imagick on Apache/PHP

This guide will tell you how to install the `ImageMagick` PHP module for Apache

I'm not really sure where exactly this should go, as it's more of a server thing, but only for Linux servers running something like a LAMP stack. Anyway, it's where I hope to find it if I look again, so we'll see.

## Step 1 - Install ImageMagick

This step assumes you're already running something like a LAMP stack with `PHP5+` if you're not, go do that first I guess.

```bash
$ sudo apt-get install imagemagick
```

## Step 2 - Install the Imagick PHP Extension

This will let our PHP code use the library

```bash
$ sudo apt-get install php5-imagick
```

## Step 3 - Restart Apache

For me, I usually do this thru webmin or something cuz I'm working on more than one thing at a time (bad, I know) but if you need to do it from the CLI

```bash
$ service apache2 reload
```

and then this little trick is quite handy to make sure it's loaded properly

```bash
$ php -m | grep imagick

# prints the following to screen
imagick
```

## Step 4 - Profit

That should be it. If you have any more trouble than that I feel bad - I know how annoying server stuff is. But this worked for me!

I'll post stuff here as I encounter it (that's the point of the Hitchhiker's Guides) and if you notice anything or have any FAQs or whatnot just submit a PR.
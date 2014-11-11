Fix: Yoast SEO conflict with PageBuilder
=========================

# Problem
Yoast SEO-Plugin does not work together with Siteorigin's PageBuilder. This repository provides a quickfix of the problem described [in this issue](https://siteorigin.com/thread/page-builder-seo-by-yoast-and-visual-editor-not-working-together/) and solved at this [tennis page](http://tclengfeld.de).

# Problem definition
Yoast SEO analyses the source code of a page or post. After editing a page with SiteOrigin's PageBuilder, the source code of the page gets generated doing a HTTP-request on this page because of proper exception handling.
If the base url of Wordpress is not known by the webserver, the source code cannot be generated. This should only be a problem during the initial creation of the Wordpress-blog, if you change your local hosts file.

## Fix 1
Just edit the hostfile of the webserver
```
# /etc/hosts
...
127.0.0.1 tclengfeld.de
...
```

## Fix 2
If you aren't allowed to edit the hosts-file, edit `copy.php` of the pagebuilder-plugin and put a real existing base url on line ~33
```
# this request e.g. http://tclengfeld.de/impressum is not known by the server. So change this 
$request = wp_remote_post( admin_url('admin-ajax.php?action=siteorigin_panels_get_post_content'),
# to the existing url of your hosting e.g. http://tcl.adhara.uberspace.de/impressum
$request = wp_remote_post( 'http://tcl.adhara.uberspace.de/wp-admin/admin-ajax.php?action=siteorigin_panels_get_post_content',
```
# Warning
The second fix is NOT UPDATEABLE but does not break Wordpress ^^. DO NOT CHANGE YOUR BASE_URL TO A CORRECT ONE WHILE PREPARING YOUR SITE. It is so hard to change the base-url of Wordpress a correct way. The Base-url gets duplicated almost everywhere, even in serialized strings. So please, always start a new Wordpress site with faking your lokal DNS via hosts-file.

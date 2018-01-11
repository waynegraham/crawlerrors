# Crawl Errors

https://aaronaddleman.com/articles/rewritemap-mod-rewrite-with-apache/

> If this gets too large (and slowing down), convert the txt map to dbm

```
httxt2dbm -i /etc/apache2/map.txt -o /etc/apache2/map_db
```

And update the `/etc/apache2/sites-available/wordpress.clir.org-le-ssl.conf` with

```
RewriteMap urlmap dbm:/etc/httpd/mod_rewrites/map_redirct.map
```

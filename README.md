# cloudflare-waf-rules
My Cloudflare WAF rules to protect websites

## Block HTTP POSTs from Tor. This often results in contact form spam.

`(http.request.method eq "POST" and ip.geoip.country eq "T1")`

## Block access to the default Wordpress login page

`(http.request.uri.path contains "/wp-login.php")`

## Block cookie-less access to the Wordpress admin pages

`(http.request.uri.path eq "/wp-admin" or http.request.uri.path eq "/wp-admin/") and not http.cookie contains "wordpress_logged"`

## Block access to archives and backups in folders other than Wordpress ones

`not http.request.uri.path contains "/wp-" and (http.request.uri.path contains ".zip" or http.request.uri.path contains ".sql" or http.request.uri.path contains ".gz" or http.request.uri.path contains ".bak" or http.request.uri.path contains ".tar")`

## Block access to PHP code in the Wordpress wp-content folder

`(http.request.uri.path contains "/wp-content/" and http.request.uri.path contains ".php")`

## Block ASP access to a PHP site

`(http.request.uri.path contains ".asp")`

## Block access to the Wordpress config file or potential clones and backups

`(http.request.uri.path contains "/wp-config")`

## Block Wordpress XMLRPC POST floods

`(http.request.uri.path contains "/xmlrpc.php" and http.request.method eq "POST")`

## Block access to anything other than existing assets on a very simple static page

`!((http.request.uri.path eq "/") or (http.request.uri.path eq "/index.html") or (http.request.uri.path eq "/en") or (http.request.uri.path eq "/en/") or (http.request.uri.path eq "/en/index.html") or (http.request.uri.path eq "/robots.txt") or (http.request.uri.path contains "/feed") or (http.request.uri.path eq "/style.css") or (http.request.uri.path eq "/favicon.ico") or (http.request.uri.path contains "/images/") or (http.request.uri.path contains "/sitemap"))`

## Block unexpected HTTP request methods on a very simple static page

`(http.request.method in {"POST" "PUT" "DELETE" "PATCH"})`

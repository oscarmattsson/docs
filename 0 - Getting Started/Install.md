- [Recommended](#recommended)
- [Manual](#manual)
- [Virtualhost](#virtualhost)
- [Nginx configuration](#nginx-configuration)
- [IIS with URL Rewrite module installed](#iis-with-url-rewrite-module-installed)


#### Recommended
The framework is located on [Packagist](https://packagist.org/packages/nova-framework/framework).

You can install the framework from a terminal by using:

```
composer create-project nova-framework/framework foldername
```

The foldername is the desired folder to be created, if left off then a folder called framework will be created.

#### Manual
- Place the contents of `public` into your public folder (`.htaccess` and `index.php`)
- Navigate to your project in a terminal and type `composer install` to initiate the composer installation.
- Edit `public/.htaccess` to set the rewritebase if running on a sub folder, otherwise a single `/` will do.
- Edit `app/Config.example.php` and change the `SITEURL` and `DIR` constants. The `DIR` path is relative to the project url for example `/` for on the root or `/foldername/` when in a folder. Also change other options as desired. Rename file to `Config.php`
- Set a 32 character `ENCRYPT_KEY` by using the CLI tool. You can do this by typing `php nova make:key` in your command line / console. Alternatively, you can use the following tool: http://jeffreybarke.net/tools/codeigniter-encryption-key-generator/

For a video example see [http://novacasts.com/courses/nova-fundamentals/install](http://novacasts.com/courses/nova-fundamentals/install)

---

## VirtualHost

Navigate to: 
```` 
<path to your xampp installation>\apache\conf\extra\httpd-vhosts.conf
````

and uncomment:

````
NameVirtualHost *:80
````

Then add your VirtualHost to the same file at the bottom:

````
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot "C:\xampp\testproject\public"
    ServerName testproject.dev

    <Directory "C:\xampp\testproject\public">
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
````

Finally, find your hosts file and add:

````
127.0.0.1       testproject.dev
````

You should then have a virtual host setup, and in your web browser, you can navigate to testproject.dev to see what you are working on.

---

## Nginx configuration

No special configuration, you only need to configure Nginx and PHP-FPM.

````
server {
  listen 80;
  server_name yourdomain.tld;

  access_log /var/www/access.log;
  error_log  /var/www/error.log;

  root   /var/www;
  index  index.php index.html;

  location = /robots.txt {access_log off; log_not_found off;}
  location ~ /\\. {deny all; access_log off; log_not_found off;}
  location / {
    try_files $uri $uri/ /index.php?$args;
  }

  location ~ \.php$ {
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
}
````

---

## IIS with URL Rewrite module installed

[http://www.iis.net/downloads/microsoft/url-rewrite](http://www.iis.net/downloads/microsoft/url-rewrite)

For IIS the htaccess needs to be converted to web.config:

````
<configuration>
    <system.webserver>
        <directorybrowse enabled="true"/>
        <rewrite>
            <rules>
                <rule name="rule 1p" stopprocessing="true">
                    <match url="^(.+)/$"/>
                    <action type="Rewrite" url="/{R:1}"/>
                </rule>
                <rule name="rule 2p" stopprocessing="true">
                    <match url="^(.*)$"/
                    <action type="Rewrite" url="/index.php?{R:1}" appendquerystring="true"/>
                </rule>
            </rules>
        </rewrite>
    </system.webserver>
</configuration>
````
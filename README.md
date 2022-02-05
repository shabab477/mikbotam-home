# Mikbotamv1.8 - Installation instructions

#### Upload project to server

- With `scp` or `git clone` command upload this repository to server
- All the files of this repository should be located at `/var/www/html/mikbotam`
- Give user permission with `sudo chown -R $USER:$USER /var/www/html/mikbotam`

#### Install sqlite

- `sudo apt install php-sqlite3`

#### Update apache2

- `cd` to `/etc/apache2`. This is default folder for the web server configuration
- `cd` to `/etc/apache2/sites-available`.
- Create and configure the site config:
    1. `touch mikbotam.conf`
    2. Copy paste the following config:
    ```
    Alias /mikbotam /var/www/html/mikbotam

    <Directory /var/www/html/mikbotam>
        Options +FollowSymLinks
        AllowOverride None
        <IfVersion >= 2.3>
        Require all granted
        </IfVersion>
        <IfVersion < 2.3>
        Order Allow,Deny
        Allow from all
        </IfVersion>

    AddType application/x-httpd-php .php

    <IfModule mod_php.c>
        php_flag magic_quotes_gpc Off
        php_flag short_open_tag On
        php_flag register_globals Off
        php_flag register_argc_argv On
        php_flag track_vars On
        # this setting is necessary for some locales
        php_value mbstring.func_overload 0
        php_value include_path .
    </IfModule>

    DirectoryIndex index.php
    </Directory>
    ``` 
    
- Create `symlink` to the config from previous step at `sites-enabled` folder.
    
    1. `cd /etc/apache2/sites-enabled`
    2. `ln -s ../sites-available/mikbotam.conf mikbotam.conf`

- Load site to apache2: `sudo a2ensite mikbotam.conf`
- Restart or reload apache2: `sudo systemctl restart apache2`
# Update Source
```sh
sudo add-apt-repository ppa:ondrej/php5
sudo apt-get update
```

# Install PHP and Extentions
```sh
sudo apt-get install php5-cgi php5-fpm php5-dev
```

# Install Nginx
```sh
sudo apt-get install nginx
```

# Modify 'default' file like below in **/etc/nginx/sites-enabled**, uncomment the php configuration part
```text
server {
  root /home/workshop/output/vincent/php;
  index index.html index.htm index.php;
  server_name localhost;

  location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
  　　# # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
  　　#
  　　# # With php5-cgi alone:
  　　# fastcgi_pass 127.0.0.1:9000;
  　　# # With php5-fpm:
  　　fastcgi_pass unix:/var/run/php5-fpm.sock;
  　　fastcgi_index index.php;
  　　include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  }
}
```

# Restart Nginx
```sh
service nginx restart
```

# Ensure that PHP FPM listen with socket
```sh
vi /etc/php5/fpm/pool.d/www.conf
```
Find the listen configuration, and modify it as below:
```text
listen = /var/run/php5-fpm.sock
```

# Restart PHP FPM
```sh
service php5-fpm restart
```

# Verify it works
Write the text below in the index.php file of nginx root folder you set in default file
```php
<?php
  phpinfo();
?>

Access http://localhost/index.php to see the configuration of your PHP
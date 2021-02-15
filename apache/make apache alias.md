# How to Make an Alias in Apache

1. Open `/etc/apache2/sites-enabled/<your url>`

2. Inside the VirtualHost tags add a line like: `Alias <url accessor> <directory path>`

- For example if you wanted `example.com/pictures` to point to `/www/pictures`
the command would be `Alias '/pictures' '/www/pictures'`

- If I want specific permissions you can add a `<Directory>` tag for that directory

3. Restart apache with `sudo apachectl restart` and test your new alias.

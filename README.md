# Set Up Apache Virtual Hosts on Ubuntu 14.04 LTS

## Prerequisites
### Apache2 installed 
you can get Apache installed on your machine  through **apt-get** :
```
sudo apt-get update
sudo apt-get install apache2
```

1. Create or move your project directory to your directory root server __/var/www/__ Or __/var/www/html__ we will work the second one in this tutorial.
 For instance: 
 ```
sudo mkdir -p /var/www/html/example
```

2. Create Demo page :
- Create index.html file
```
nano /var/www/html/example/index.html
```
Or use sublime if you are not familiar with nano.
```
subl /var/www/html/example/index.html
```
- Put same basic html in it.
Copy and paste the following.
```
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success!  The example.com virtual host is working!</h1>
  </body>
</html>
```
Save and close the file when you are finished.

3. Create the Virtual Host File
- Start by copying the file for the domain:
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```
- Open the new file in your editor with root privileges i will use sublime but you can use nano:
```
sudo subl /etc/apache2/sites-available/example.com.conf
```
- The file will look something like this (I've removed the comments here to make the file more approachable):
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- We need to change the following directives.
    1. ``ServerAdmin admin@example.com`` : directive to an email that the site administrator can receive emails through.
    2. ``ServerName example.com`` and ``ServerAlias www.example.com``, the domain name and the alias.
    3. Last directive is DocumentRoot ``DocumentRoot /var/www/html/example``: document root for this domain.

In total, our virtualhost file should look like this:
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Save and close the file.

4. Enable the New Virtual Host Files
``` 
sudo a2ensite example.com.conf
```
When you are finished, you need to restart Apache to make these changes take effect:
```
sudo service apache2 restart
```
5. Set Up Local Hosts File : Add our new host to hosts file.
- Open hosts file.
``` 
sudo subl /etc/hosts
```
you will probably see something like this 
```
127.0.0.1	localhost
127.0.1.1	YOURUER-PC

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
- Add the following line ``127.0.1.1	example.com`` after ``127.0.1.1	YOURUER-PC``
Now the hosts file looks like this 
```
127.0.0.1	localhost
127.0.1.1	YOURUER-PC
127.0.1.1	example.com

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
6. Test your Results, And enjoy it :+1: 
you can test your setup easily by going to the domains that you configured in your web browser : 
``http://example.com`` 
You should see a page that looks like this:

# Success!  The example.com virtual host is working!

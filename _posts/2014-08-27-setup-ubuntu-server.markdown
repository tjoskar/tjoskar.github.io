---
layout: post
title:  "Setup ubuntu server"
date:   2014-08-27 10:18:00
categories: ubuntu server
---

A few, initial, steps to setup a ubuntu (14.04) server.

Make sure you have the latest software updates
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get upgrade
{% endhighlight %}

Create a new user and add sudo privileges
{% highlight bash %}
$ sudo adduser newUserName
$ sudo usermod -aG sudo newUserName
{% endhighlight %}

Add your SSH key
{% highlight bash %}
$ mkdir ~/.ssh && cd ~/.ssh
$ nano authorized_keys # Insert your public key
$ cd ..
$ chmod 700 .ssh
$ chmod 644 authorized_keys
{% endhighlight %}

Update SSH config
{% highlight bash %}
$ sudo nano /etc/ssh/sshd_config
{% endhighlight %}
Example config:
{% highlight bash %}
Port 500
Protocol 2
PermitRootLogin no
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile  %h/.ssh/authorized_keys
IgnoreRhosts yes
AllowUsers newUserName # Change to your username!
PermitEmptyPasswords no
PasswordAuthentication no
{% endhighlight %}

Enable iptables.
{% highlight bash %}
# Explicitly accepts your current SSH connection
$ sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
# Accepts 500, 80
$ sudo iptables -A INPUT -p tcp --dport 500 -j ACCEPT
$ sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
# Loop back
$ sudo iptables -I INPUT 1 -i lo -j ACCEPT
# Drop all other
$ sudo iptables -P INPUT DROP
{% endhighlight %}
Do the same for ip6
{% highlight bash %}
$ sudo ip6tables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
$ sudo ip6tables -A INPUT -p tcp --dport 500 -j ACCEPT
$ sudo ip6tables -A INPUT -p tcp --dport 80 -j ACCEPT
$ sudo ip6tables -I INPUT 1 -i lo -j ACCEPT
$ sudo ip6tables -P INPUT DROP
{% endhighlight %}

List the rules
{% highlight bash %}
$ sudo iptables -L --line-numbers
$ sudo ip6tables -L --line-numbers
{% endhighlight %}

Install LAMP
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install apache2
$ sudo apt-get install mysql-server php5-mysql
$ sudo mysql_install_db
$ sudo mysql_secure_installation
$ sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
$ sudo apt-get install php5-curl php5-gd php5-json php5-imagick
{% endhighlight %}

Update apache.
Make apache to first search for index.php files. Change <code>/etc/apache2/mods-enabled/dir.conf</code> from:
{% highlight apache %}
<IfModule mod_dir.c>
    DirectoryIndex index.html ... something
</IfModule>
{% endhighlight %}
to:
{% highlight apache %}
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html
</IfModule>
{% endhighlight %}

Hide Apache Version and OS Identity from errors by edit <code>/etc/apache2/apache2.conf</code>
{% highlight apache %}
ServerSignature Off
ServerTokens Prod
<Directory /var/www/html>
    Options -Indexes
</Directory>
{% endhighlight %}

Install Mod Security
{% highlight bash %}
$ sudo apt-get install libapache2-modsecurity
# You should now see something like 'security2_module (shared)'
$ apachectl -M | grep --color security
# Create and change the config file
$ mv /etc/modsecurity/modsecurity.conf{-recommended,}
$ nano /etc/modsecurity/modsecurity.conf
{% endhighlight %}

Example config:
{% highlight bash %}
SecRuleEngine On
SecResponseBodyAccess Off
SecRequestBodyLimit 5107200
{% endhighlight %}

Restart apache
{% highlight bash %}
$ sudo service apache2 restart
{% endhighlight %}

Update PHP config (for apache)
{% highlight bash %}
$ sudo nano /etc/php5/apache2/php.ini
{% endhighlight %}

Example config
{% highlight bash %}
expose_php=Off
display_errors=Off
log_errors=On
file_uploads=Off
allow_url_fopen=Off
allow_url_include=Off
max_execution_time =  30
max_input_time = 30
memory_limit = 40M
{% endhighlight %}

Enable mcrypt for PHP-CLI by adding <code>extension=mcrypt.so</code>
{% highlight bash %}
sudo nano /etc/php5/cli/php.ini
{% endhighlight %}

This was just a basic installation, you will still have to configure your system after your needs.

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com

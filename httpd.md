##### Install Apache HTTPD

[Rhel-Install](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/deploying_web_servers_and_reverse_proxies/setting-apache-http-server_deploying-web-servers-and-reverse-proxies#apache-intro_setting-apache-http-server
)


```dnf install httpd```

```firewall-cmd --permanent --add-port=80/tcp```

```firewall-cmd --reload```

```systemctl enable --now httpd```

```systemctl status httpd```

```systemctl stop httpd```

```systemctl restart httpd```

###### Apache configuration files

The main configuration file
```
/etc/httpd/conf/httpd.conf
```
###### Hosts virtuais

Edit the file: /etc/httpd/conf/httpd.conf

All settings in the directive are specific for this virtual host. <VirtualHost *:80>

```DocumentRoot``` sets the path to the web content of the virtual host.

```ServerName``` sets the domains for which this virtual host serves content.

To set multiple domains, add the parameter to the configuration and specify the additional domains separated with a space in this parameter. ```ServerAlias```

```CustomLog``` sets the path to the access log of the virtual host.

```ErrorLog``` sets the path to the error log of the virtual host.

```
<VirtualHost *:80>
    DocumentRoot "/var/www/site01.com/"
    ServerName site01.com
    CustomLog /var/log/httpd/site01.com_access.log combined
    ErrorLog /var/log/httpd/site01.com_error.log
</VirtualHost>

```




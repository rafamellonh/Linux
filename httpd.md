##### Install Apache HTTPD

[Rhel](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/deploying_web_servers_and_reverse_proxies/setting-apache-http-server_deploying-web-servers-and-reverse-proxies#apache-intro_setting-apache-http-server
).


```dnf install httpd```

```firewall-cmd --permanent --add-port=80/tcp```

```firewall-cmd --reload```

```systemctl enable --now httpd```

```systemctl status httpd```

```systemctl stop httpd```

```systemctl restart httpd```

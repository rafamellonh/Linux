##### Install Apache HTTPD
```dnf install httpd```

```firewall-cmd --permanent --add-port=80/tcp```

```firewall-cmd --reload```

```systemctl enable --now httpd```

```systemctl status httpd```

```systemctl stop httpd```

```systemctl restart httpd```

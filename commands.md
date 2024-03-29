##### Commands

* ``` Stat ``` : Mostra informações de status de um arquivo ou sistema de arquivos. Dentre as informações estão o Tamanho, Blocos, Permissões de Acesso, Data e Hora de último acesso,
Data e Hora de última modificação, etc. e caminho do diretório atual.

``` Stat /docker

File: '/docker'
Size: 4096      	Blocks: 8          IO Block: 4096   directory
Device: 10301h/66305d	Inode: 123456      Links: 2
Access: (0755/drwxr-xr-x)  Uid: ( 1000/  user)   Gid: ( 1000/  user)
Access: 2023-01-01 12:34:56.789012345 +0000
Modify: 2023-01-01 12:34:56.789012345 +0000
Change: 2023-01-01 12:34:56.789012345 +0000
Birth: - 2023-01-01 12:34:50.789012345 +0000

```

* ``` ln -s  ``` : cria links (atalhos)  ``` ln -s /var/log /opt/log/ ```

* TIMEZONE

```
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone America/New_York
timedatectl
TZ="America/New_York" date   : verifica o timezone
timedatectl list-timezones | grep -i "Sao"
```

* yum - rpm
```
rpm -qa   (lista todos apps instalados)
rpm -qa | grep htop
rpm -qi bash   (verifica o status do app instalado)
```

* services
  
` systemctl list-units --type=service --state=active > services.txt `


* certificat
```
openssl x509 -in NameCert.crt -text -noout
```





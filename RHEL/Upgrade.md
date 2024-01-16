##### Check kernel version

```
uname -r

uname -a

uname -mrs

cat /proc/version
```


##### Show information about update advisories, run:

```
sudo yum updateinfo

[root@localhost rafael]# yum updateinfo
Updating Subscription Management repositories.
Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)                             13 kB/s | 4.5 kB     00:00
Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)                             13 MB/s |  58 MB     00:04
Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)                                16 kB/s | 4.1 kB     00:00
Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)                                20 MB/s |  65 MB     00:03
Last metadata expiration check: 0:00:08 ago on Tue 16 Jan 2024 01:19:26 PM EST.
Updates Information Summary: available
    18 Security notice(s)
         7 Important Security notice(s)
        10 Moderate Security notice(s)
         1 Low Security notice(s)
     8 Bugfix notice(s)
```


##### These commands are useful

```
sudo yum check-update

sudo yum check-update | more

sudo yum check-update | grep bash

sudo yum check-update
```


##### Show information about update advisories, run:

##### Reboot the system if kernel was updated by typing ```sudo reboot ``` command.

```
sudo yum update

```

##### One can only apply security related updates to the machines, run:

```
sudo yum --security update
```

##### How do I update a single package?
```
sudo yum update pkg_name
sudo yum update bash

```

##### It is also possible to install all updates except kernel and bash packages as follows:

```
sudo yum -x 'kernel*' -x 'bash*' update
```



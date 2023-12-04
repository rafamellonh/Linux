###### Install Docker CE and Docker compose on RHEL 8 or RHEL 9 using yum
Before you begin: You need a valid Red Hat subscription and account credentials to perform these actions.

To install Docker CE and Docker compose on RHEL 8:

###### Add a subscription on a RHEL 8 system. To do that, complete the following steps:
Register the system:

```sudo subscription-manager register ```

This command prompts you to enter your Red Hat account credentials. After successful registration, the system is associated with your Red Hat account.
Refresh:

``` sudo subscription-manager refresh ```

This command manually refreshes the local system's subscription information.

Attach a subscription:

```sudo subscription-manager attach --auto ```

This command automatically attaches available subscriptions to your system.

For more details on adding a subscription on a RHEL 8 system, see https://access.redhat.com/solutions/253273

Add the external repository by running the following command.

``` sudo yum config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo ```

```sudo yum -y install docker-ce --allowerasing```

Start and enable the docker daemon

```sudo systemctl enable --now docker```

Confirm whether the daemon is active by running this command:

```systemctl is-active docker```

Install docker-compose globally.

Download the binary file from the project’s GitHub page:

```curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose```

After the binary file is downloaded, move it to the /usr/local/bin folder, and then make it executable:

```sudo mv docker-compose /usr/local/bin && sudo chmod +x /usr/local/bin/docker-compose```

```sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose ```

### Log into Browser UI
From either Firefox or Chrome, log into the DC/OS Browser UI

### SSH into "DC/OS Bootstrap Node"
From your favorite SSH client (Termius, puTTy, iTerm, OSX Terminal, etc), connect to your DC/OS Bootstrap Node
```
ssh <username>@<bootstrapNodePublicIpAddress>
```

Install DC/OS CLI

1.  From the Browser UI, click the "Cluster Name" in the upper left-hand corner of the screen
2.  From the drop-down list, select "Install CLI"
3.  In the window that appears, select the OS type to which you will be installing (As it will be installed on your BootStrap Node, select Linux)
4.  Copy the commands as 1 large block of text.  It will resemble the following, except it include settings for your sopecific cluster
```
[ -d /usr/local/bin ] || sudo mkdir -p /usr/local/bin && 
curl https://downloads.dcos.io/binaries/cli/linux/x86-64/dcos-1.11/dcos -o dcos && 
sudo mv dcos /usr/local/bin && 
sudo chmod +x /usr/local/bin/dcos && 
dcos cluster setup https://192.168.1.211 && 
dcos
```
5.  Paste the copied text into your SSH session (the one where you connected to the DC/OS Bootstrap Node)
6.  Accept requests and enter userIDs and Passwords (first the password for the pootstrap node, Second the User ID and password for the DC/OS cluster account





### Install Enterprise-CLI
The Enterprise CLI, available only in Enterprise DC/OS provides access to Security and Backup Command Line interfaces
```
dcos package install --yes dcos-enterprise-cli
```



SSH into Bootstrap Node

Install CLI


Install Enterpriuse CLI





Create Service Account

Create Jenkins Service Account
Create GitLab Service Account

### Install Enterprise-CLI
```
dcos package install --yes dcos-enterprise-cli
```

### Create Public-Private Key Pair
```
dcos security org service-accounts keypair jenkins-private-key.pem jenkins-public-key.pem
```

### Create and Verify a New Service Account "jenkins-principal"
```
dcos security org service-accounts create -p jenkins-public-key.pem -d "Jenkins service account" jenkins-principal
dcos security org service-accounts show jenkins-principal
```

### Create and Verify a new Secret "jenkins/jenkins-secret"
```
dcos security secrets create-sa-secret jenkins-private-key.pem jenkins-principal jenkins/jenkins-secret
dcos security secrets list /
```

### Delete Key from Local System
```
rm -rf jenkins-private-key.pem
```

### Retrieve the DC/OS CA Bundle
```
curl -k -v $(dcos config show core.dcos_url)/ca/dcos-ca.crt -o dcos-ca.crt
```

### Create Necessary Permissions
```
curl -X PUT --cacert dcos-ca.crt -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/acs/api/v1/acls/dcos:mesos:master:task:user:nobody -d '{"description":"Allows Linux user nobody to execute tasks"}' -H 'Content-Type: application/json'

curl -X PUT --cacert dcos-ca.crt -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/acs/api/v1/acls/dcos:mesos:master:framework:role:* -d '{"description":"Controls the ability of jenkins-role to register as a framework with the Mesos master"}' -H 'Content-Type: application/json'
```

### Grant Permissions and Allowed acrions to Service account
```
curl -X PUT --cacert dcos-ca.crt -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/acs/api/v1/acls/dcos:mesos:master:framework:role:*/users/jenkins-principal/create

curl -X PUT --cacert dcos-ca.crt -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/acs/api/v1/acls/dcos:mesos:master:task:user:nobody/users/jenkins-principal/create
```

### Create "config.json" File
```
{
  "security": {
    "secret-name": "jenkins/jenkins-secret"
  },
  "service": {
    "user": "nobody"
  }
}
```

### Install jenkins
```
dcos package install --options=config.json jenkins
```
Go to the Services section of the Enterprise DC/OS Web UI.  this will 

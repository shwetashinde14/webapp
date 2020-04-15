mirrored from https://github.com/sebsto/webapp

# This is a sample Web Application to use during Continuous Integration demos.

## Build Instruction

```
mvn clean package
```

## Deploy instruction

Deploy ```target/WebApp.war``` on Tomcat
 
## CICD
 
### Jenkinsfile based CICD

* Jenkins input: Tomcat server IP

* ssh key stored in Jenkins Credentials with name 'gcp_ssh_key' ('gcp_ssh_key' is just a naming convention. vm can be hosted on any platform. Modify the Jenkinsfile to name it differently)

* uses scp to deploy WebApp.war on the Tomcat server at location /opt/tomcat/webapps/

* Additional required Jenkins plugins: AnsiColor, Pipeline Utility Steps

## Tomcat Server setup:

* Tomcat version: 8.X.XX

* Tomcat location: /opt/tomcat

### note: make sure correct permissions are set on /opt/tomcat, when creating Tomcat server on the target machine. For reference on how to setup Tomcat on a GCP instance: https://github.com/praddevops/ansible_install_packages 

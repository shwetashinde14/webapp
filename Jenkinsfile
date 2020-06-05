pipeline {
    agent any

    parameters {
       string(name: 'TomcatHost', defaultValue: '', description: 'TOMCAT SERVER IP')  
       string(name: 'RemoteHost_UserName', defaultValue: 'jenkins', description: 'enter the username you want to connect as to the remote host. Ex: For AWS EC2 Amazon Linux 2 AMI: ec2-user')  
    }

    options {
        ansiColor('xterm')
        timestamps()
    }

    environment {
      SECRETKEYFILE = credentials('ssh_key')
      VERSION = readMavenPom().getVersion()
      REMOTEUSERNAME = "${params.RemoteHost_UserName}" 
    }

    stages {
        
	//if sonar is configured, 'mvn clean test sonar:sonar <sonar options>'
        stage('unit tests and coverage') {
            steps {
             sh "mvn test"
            }
        }
	
        stage('Build') {
            steps {
             sh "mvn clean install"  
            }
        }
        

        stage('Deploy') {
            steps {
              sh """
              set +x
              cat ${SECRETKEYFILE} > ssh_key
              chmod 400 ssh_key
              scp -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -q target/WebApp.war ${REMOTEUSERNAME}@${params.TomcatHost}:/tmp/
	      ssh -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${REMOTEUSERNAME}@${params.TomcatHost} sudo mv /tmp/WebApp.war /opt/tomcat/webapps/
              ssh -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${REMOTEUSERNAME}@${params.TomcatHost} sudo /opt/tomcat/bin/startup.sh
              """
            }
        }
	}
    post { 
        always {
          cleanWs()
        }
    }
}

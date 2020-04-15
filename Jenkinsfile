pipeline {
    agent any

    parameters {
       string(name: 'TomcatHost', defaultValue: '', description: 'TOMCAT SERVER IP')   
    }

    options {
        ansiColor('xterm')
        timestamps()
    }

    environment {
      SECRETKEYFILE = credentials('gcp_ssh_key')
      VERSION = readMavenPom().getVersion()
    }

    stages {
           
        stage('Build') {
            steps {
             sh "mvn clean install"  
            }
        }
        
        //if sonar is configured, 'mvn clean test sonar:sonar <sonar options>'
        stage('unit tests and coverage') {
            steps {
             sh "mvn test"
            }
        }

        stage('Deploy') {
            steps {
              sh """
              set +x
              cat ${SECRETKEYFILE} > ssh_key
              chmod 400 ssh_key
              scp -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -q target/WebApp.war ${params.TomcatHost}:/tmp/
	      ssh -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null jenkins@${params.TomcatHost} sudo mv /tmp/WebApp.war /opt/tomcat/webapps/
              ssh -i ssh_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null jenkins@${params.TomcatHost} sudo /opt/tomcat/bin/startup.sh
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

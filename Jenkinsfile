pipeline {
    agent {
        node {
            label 'built-in'
            customWorkspace '/mnt/jenkins'
        }
    }
    stages {
        stage ('new_folder') {
            steps {
                sh 'mkdir Dee-copy-1'
            }
        }
    }
}

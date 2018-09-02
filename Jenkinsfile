pipeline {
    agent {
        label 'mvn'
    }

    stages {
        stage ('Build') {
            steps {
                sh '/usr/local/bin/mvn package' 
            }
            post {
                success {
                    junit '**/surefire-reports/*.xml' 
                }
            }
        }
    }
}


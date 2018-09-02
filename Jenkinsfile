pipeline {
    agent {
        label 'mvn'
    }

    stages {
        stage ('Build') {
            steps {
                sh 'mvn package' 
            }
            post {
                success {
                    junit '**/surefire-reports/*.xml' 
                }
            }
        }
    }
}
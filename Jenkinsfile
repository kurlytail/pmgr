pipeline {
    agent {
        label 'mvn'
    }
    
    parameters {
        string(defaultValue: "0.0", description: 'Build version prefix', name: 'BUILD_VERSION_PREFIX')
        string(defaultValue: "", description: 'Build number offset', name: 'BUILDS_OFFSET')
    }

    stages {
        stage('Prepare env') {
            steps {
                script {
                    loadLocalLibrary()
                    env['MAVEN_VERSION_NUMBER'] = getMavenVersion 'pmgr/job/master', params.BUILD_VERSION_PREFIX, params.BUILDS_OFFSET
                }
                
                sh '/usr/local/bin/mvn --batch-mode release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=$MAVEN_VERSION_NUMBER'
            }
        }
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


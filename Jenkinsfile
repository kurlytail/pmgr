pipeline {

    agent none
    
    parameters {
        string(defaultValue: "0.0", description: 'Build version prefix', name: 'BUILD_VERSION_PREFIX')
        string(defaultValue: "", description: 'Build number offset', name: 'BUILDS_OFFSET')
    }

    stages {
        stage('Prepare env') {
            agent {
                label 'master'
            }
            
            steps {
                script {
                    loadLibrary()
                    env['MAVEN_VERSION_NUMBER'] = getMavenVersion 'kurlytail/pmgr/master', params.BUILD_VERSION_PREFIX, params.BUILDS_OFFSET
                }
            }
        }
        
        stage ('Build') {
            agent {
                label 'mvn'
            }
            
            steps {
                sh '/usr/local/bin/mvn --batch-mode release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=$MAVEN_VERSION_NUMBER'
                sh '/usr/local/bin/mvn package' 
                script {
                    if(env.BUILDING_QA_CANDIDATE == 'true') {
                        sh "curl --upload-file pmgr-app/target/pmgr-app.war 'http://admin:password@localhost:9080/manager/text/deploy?path=/pmgr-app&update=true'"
                    }
                }
            }
            
            post {
                always {
                    archiveArtifacts artifacts: 'target/**/*.war', fingerprint: true
                }
                success {
                    junit '**/surefire-reports/*.xml' 
                }
            }
        }
    }
}


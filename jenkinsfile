pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time:30, unit: 'MINUTES')
       disableConcurrentBuilds();
    }
    environment {
        appVersion = ''
    }
    stages {
        stage('read the package.json'){
            steps{
                script {
                    withAWS(credentials: 'aws cred', region: 'us-east-1') {
                        def packageJson = readJSON file: 'package.json'
                        appVersion = packageJson.version
                        echo "application version is ${appVersion}"
                    }
                    
                }
            }
        }
    }
}
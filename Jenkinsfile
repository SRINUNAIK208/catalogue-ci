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
        region = 'us-east-1'
        Account_ID = '388343452532'
        project = "roboshop"
        component = "catalogue"
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
        stage('install dependency'){
            steps{
                sh """
                   npm install
                """
            }
        }
        stage('Build docker image'){
            steps{
                withAWS(credentials: 'aws cred', region: 'us-east-1'){
                    sh """
                        aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .
                        docker push ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                    """
                }
            }
        }


    }
   
}
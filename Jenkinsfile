pipeline {
    // these are pre-build section
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = "latest"
        ACC_ID = "678511327499"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout (time:10 , unit:'MINUTES')
        disableConcurrentBuilds()
    }
    // these are build section
    stages {
        stage('Read Version'){
            steps {
               script {
                   def packageJSON = readJSON file: 'package.json'
                   env.appVersion = packageJSON.version
                   echo "app version: ${env.appVersion}"
               }  
            }
        }
        stage('Install Dependencies'){
            steps {
                script {
                    sh """
                       npm install
                    """
                }
            }
        }
        stage('Unit Test') {
            steps {
                script{
                    sh """
                       echo 'No unit tests configured - skipping Unit Test stage'
                    """
                }
            }
        }
        stage('Build Image'){
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                }
            }
        }
    }    
    // these are post-build section
    post{
        always {
            echo 'I will always say hello again !'
            cleanWs()
        }
        success {
            echo 'I will run if success'
        }
        failure {
            echo 'I will run if failure'
        }
        aborted {
            echo 'pipeline is aborted'
        }
    }
}
pipeline {
    // these are pre-build section
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = ""
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
                   appVersion = packageJSON.Version
                   echo "app version : ${appVersion}"
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
        stage('Build Image'){
            steps {
                script {
                    sh """
                       docker build -t catalogue : ${appVersion} .
                       docker images
                    """
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
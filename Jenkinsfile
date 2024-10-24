pipeline {
    agent any
    
    environment {
        TOMCAT_URL = 'http://your-tomcat-server-ip:8080/manager/text'
        CREDENTIALS_ID = 'tomcat-credentials' // ID of the Jenkins credential you added
        APP_NAME = 'myapp' // Name of your application
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/username/repository.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = "target/${APP_NAME}.war"
                    
                    deploy adapters: [
                        tomcat8(credentialsId: CREDENTIALS_ID, url: TOMCAT_URL)
                    ], contextPath: APP_NAME, war: warFile
                }
            }
        }
    }
    
    post {
        always {
            cleanWs() // Clean the workspace after the build
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

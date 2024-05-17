pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub' // Replace with your credentials ID
        DOCKERHUB_REPO = 'dhillevajja/buildgame' // Replace with your Docker Hub repository
        IMAGE_TAG = "${env.BUILD_NUMBER}" // Use the build number as the tag
    }
    
    

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/dhille98/Boardgame.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn package"
            }
        }
        
        stage('Test and Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                junit testResults: '*/**/*.xml'
            }
        }
         stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage('sonar-QUBE') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar-cloud', installationName: 'sonar-scanner') { // You can override the credential to be used
                sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://34.125.183.239:9000/ -Dsonar.projectKey=build-game'}

            }
        }
        

        
       
    }
}

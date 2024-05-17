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
        
        
        stage('Build') {
            steps {
                script {
                    echo "Building Docker image with tag: $IMAGE_TAG"
                    sh 'docker build -t $DOCKERHUB_REPO:$IMAGE_TAG .'
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        echo "Logged in to Docker Hub"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        sh 'docker push $DOCKERHUB_REPO:$IMAGE_TAG'
                    }
                }
            }
        }
        stage(''){
            steps {
               withKubeConfig(caCertificate: '', clusterName: 'kind-kind-2', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://127.0.0.1:46473') {
               sh ' kubectl apply -f deployment.yml'
       } 
            }
        }

        
       
    }
}

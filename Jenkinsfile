pipeline {
    agent none

    stages {
        
       
                stage('git url'){
                    agent { label 'Build' }
                    steps{
                        git url: 'https://github.com/dhille98/Boardgame.git'
                        branch: 'prod'
                    }
                }
                stage('Job on Node 1') {
                    agent { label 'Build' }
                    steps {
                        script {
                            // Your build steps for node 1
                            sh 'mvn clean package'
                        }
                    }
                }

                stage('Job on Node 2') {
                    agent { label 'system-test' }
                    steps {
                        script {
                            // Your build steps for node 2
                            echo 'Running on Node 2'
                        }
                    }
                }

                stage('Job on Node 3') {
                    agent { label 'perftest' }
                    steps {
                        script {
                            // Your build steps for node 3
                            echo 'Running on Node 3'
                        }
                    }
                }
            }
        }
    

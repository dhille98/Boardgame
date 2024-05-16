pipeline{
    agent any

    stages{
        stage(git clone){
            stage('Git Checkout') {
            steps {
               git branch: 'dev',  url: 'https://github.com/dhille98/Boardgame.git'
            }
        }
        }
    }
}
pipeline {
    agent any
    stages {

        stage('build'){
            agent{
                docker{
                    image 'node:20-alpine'
                    // reuseNode true
                    // args '-u root' 
                }
            }
            steps{
                sh '''
                    ls
                    cd ./backend
                    npm install
                '''
            }
        }
        
        stage('List Files') {
            steps {
                sh 'ls -la'
                sh 'pwd'
            }
        }
    }
}

pipeline {
    agent any
    stages {

        stage('build'){
            agent{
                docker{
                    image 'node:20-alpine'
                    reuseNode true
                    args '-u root' 
                }
            }
            steps{
                sh '''
                    npm install
                '''
            }
        }

        stage('test') {
            steps {
                sh '''
                    echo "Running tests..."
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('sonarqube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        
    }
}

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kts-backend"
        DOCKER_USERNAME="kavinduorg"
        DOCKERHUB_PASS=credentials('dockerhub-pass')
    }


    stages {

        stage('Build'){
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

        stage('Test') {
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

         stage('Build Docker Image') {
            steps {
                sh '''
                docker build -f Dockerfile.prod -t $DOCKER_USERNAME/$DOCKER_IMAGE:latest .              
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                
                sh '''
                 echo $DOCKERHUB_PASS | docker login -u $DOCKER_USERNAME --password-stdin
                '''
                
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                docker push $DOCKER_USERNAME/$DOCKER_IMAGE:latest 
                '''
            }
        } 
    }

     post {
        always {
             sh '''
            docker logout || true
            docker rmi $DOCKER_USERNAME/$DOCKER_IMAGE:latest || true
            '''
        }
    }
}



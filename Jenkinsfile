pipeline {
    agent any

    environment {
        IMAGE_NAME = "prianshuamarkhalde/devsecops-nexus:${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SCANNER_HOME = tool 'SonarScanner'
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Docker Push to JFrog') {
            steps {
                script {

                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        'credentials-dockerhub'
                    ) {

                        dockerImage.push("${BUILD_NUMBER}")
                        dockerImage.push('latest')

                    }

                }
            }
        }

        stage('Deploy to Amazon EKS') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }

        success {
            echo 'Pipeline executed successfully.'
        }

        failure {
            echo 'Pipeline execution failed.'
        }
    }
}

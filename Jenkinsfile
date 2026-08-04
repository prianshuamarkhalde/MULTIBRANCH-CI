pipeline {
    agent any

    environment {
        IMAGE_NAME = "54.145.247.149:8082/devsecops-nexus:${BUILD_NUMBER}"
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

        stage('Upload Artifact to JFrog') {
            steps {
                sh '''
                    jf c use jfrog-server
                    jf rt upload "target/*.jar" "libs-release-local/"
                '''
            }
        }

        stage('Download Artifact from JFrog') {
            steps {
                sh '''
                    mkdir -p downloaded
                    jf c use jfrog-server
                    jf rt download "libs-release-local/**/*.jar" "downloaded/"
                '''
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
                    docker.withRegistry('http://54.145.247.149:8082', 'credentials-jfrog') {
                        dockerImage.push()
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

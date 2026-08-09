```groovy
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

       stage('OWASP Dependency-Check') {
        steps {
            sh '''
                echo "=== Jenkins Workspace ==="
                pwd

                echo "=== Creating report directory ==="
                mkdir -p "$WORKSPACE/dependency-check-report"

                echo "=== Checking directory ==="
                ls -ld "$WORKSPACE/dependency-check-report"

                echo "=== Running Dependency-Check ==="
                dependency-check.sh \
                    --project "DevSecOps-Nexus" \
                    --scan "$WORKSPACE" \
                    --format HTML \
                    --format XML \
                    --out "$WORKSPACE/dependency-check-report" \
                    --failOnCVSS 7
                '''
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

        stage('Trivy Image Scan') {
            steps {
                sh '''
                    trivy image \
                        --severity HIGH,CRITICAL \
                        --exit-code 1 \
                        "${IMAGE_NAME}"
                '''
            }
        }

        stage('Docker Push to DockerHub') {
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

        stage('Deploy to EKS') {
            steps {
                sh '''
                    aws eks update-kubeconfig \
                        --region us-east-1 \
                        --name my-eks-cluster

                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml

                    kubectl get pods
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
```

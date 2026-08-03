pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Update with your project build command
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test' // Or any other unit testing framework
            }
        }

        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'SonarScanner'
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '${scannerHome}/bin/sonar-scanner'
                }
            }
        }

        stage('Upload Artifact to JFrog') {
            steps {
                rtUpload(
                    serverId: 'jfrog-server',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "libs-release-local/"
                            }
                        ]
                    }'''
                )
            }
        }

        stage('Download Artifact from JFrog') {
            steps {
                rtDownload(
                    serverId: 'jfrog-server',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "libs-release-local/*.jar",
                                "target": "downloaded/"
                            }
                        ]
                    }'''
                )
            }
        }

        stage('Docker Build and Publish') {
            steps {
                script {
                    // Build Docker image using the Dockerfile
                    def dockerImage = docker.build("devsecops-nexus:${BUILD_NUMBER}", ".")

                    // Login to Artifactory Docker Registry and push image
                    docker.withRegistry('http://54.145.247.149:8082', 'credentials-jfrog') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}

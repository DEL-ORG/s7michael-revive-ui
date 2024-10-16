pipeline {
    agent any

    environment {
        CI = 'true'
        scannerHome = '/opt/sonar-scanner' // Path to Sonar Scanner
        DOCKERHUB_CREDENTIALS = credentials('del-docker-hub-auth') // Docker Hub credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git branch: 'main', 
                    credentialsId: 'github-ssh', 
                    url: 'git@github.com:DEL-ORG/s7michael-revive-ui.git'
            }
        }

        stage('Unit Test') {
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-18' // Use an appropriate Docker image with Java and Maven installed
                    args '-u root:root' // Run commands as root user inside the container
                }
            }
            steps {
                script {
                    // Run the unit tests using Maven
                    sh '''
                    cd ui
                    mvn test
                    '''
                }
            }
        }

        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:5.0.1' // Use Sonar Scanner Docker image
                }
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use the Jenkins build number as the Docker image tag
                    sh '''
                    #!/bin/bash
                    cd ui
                    docker build -t your-dockerhub-username/your-image-name:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Update and Push Helm Chart') {
            steps {
                // Switch to the second repository and update Helm chart
                script {
                    git branch: 'dev', 
                        credentialsId: 'github-ssh', 
                        url: 'git@github.com:DEL-ORG/s7michael-deployment.git'

                    // Logic to update Helm chart would go here
                }
            }
        }
    }
}

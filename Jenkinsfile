pipeline {
    agent any

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

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:latest' // Use the SonarQube Scanner Docker image
                    args '-u root:root' // Run commands as root user inside the container
                }
            }
            environment {
                SONAR_TOKEN = credentials('sonarqube-token') // Define environment variable for SonarQube token
            }
            steps {
                echo 'Running SonarQube analysis with Sonar Scanner inside Docker as root...'
                withSonarQubeEnv('Sonar') {
                    sh '''
                    /opt/sonar-scanner/bin/sonar-scanner
                    '''
                }
            }
        }

        // Additional stages can be added here
    }
}

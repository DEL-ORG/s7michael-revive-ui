pipeline {
    agent any

    environment {
        // Define SonarQube environment variables
        SONARQUBE_URL = 'http://your-sonarqube-server-url'
        SONARQUBE_TOKEN = credentials('sonarqube-token') // Use Jenkins credentials for the SonarQube token
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

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'maven:3.8.5-openjdk-18' // Use the same Docker image for SonarQube analysis
                    args '-u root:root' // Run commands as root user inside the container
                }
            }
            steps {
                script {
                    // Run SonarQube analysis using Maven
                    sh '''
                    cd ui
                    mvn sonar:sonar
                    '''
                }
            }
        }

        // Additional stages can be added here
    }
}

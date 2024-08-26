pipeline {
    agent any

    environment {
        // Define SonarQube server URL
        SONARQUBE_URL = 'http://your-sonarqube-server-url'
        // Use the Jenkins credentials ID for the SonarQube token
        SONARQUBE_TOKEN = credentials('sonarqube-token')
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
                    cd /home/automation/workspace/S7STUDENTS/s7michael/s7michael-revive-ui/ui
                    mvn test
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:latest' // Use SonarScanner Docker image
                    args '-u root:root' // Run commands as root user inside the container
                }
            }
            steps {
                script {
                    // Run SonarScanner with full path to the configuration file
                    sh '''
                    /opt/sonar-scanner/bin/sonar-scanner
                    '''
                }
            }
        }

        // Additional stages can be added here
    }
}

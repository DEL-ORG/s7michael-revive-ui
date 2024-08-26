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

        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:4.7.0' // Use Sonar Scanner Docker image
                }
            }
            environment {
                CI = 'true'
                scannerHome = '/opt/sonar-scanner' // Path to Sonar Scanner
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        // Additional stages can be added here
    }
}

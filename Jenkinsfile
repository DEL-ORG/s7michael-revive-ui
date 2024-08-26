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
                    image 'openjdk:17' // Use an appropriate Docker image with Java installed
                    args '-v /root/.m2:/root/.m2 -v /home/automation/workspace/S7STUDENTS/s7michael/s7michael-revive-ui:/workspace:rw -w /workspace --user root'
                }
            }
            steps {
                script {
                    // Run the unit tests using Maven wrapper
                    sh 'mvnw test'
                }
            }
        }

        // Additional stages can be added here
    }
}

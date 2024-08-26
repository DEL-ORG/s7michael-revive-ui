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
                    args '-v /root/.m2:/root/.m2' // Mount the Maven cache to speed up the builds
                }
            }
            steps {
                // Run the unit tests using Maven or Gradle
                sh './mvnw test' // or './gradlew test' if using Gradle
            }
        }

        // Additional stages can be added here
    }
}

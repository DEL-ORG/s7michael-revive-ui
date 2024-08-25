pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                // Replace 'https://github.com/your-repo.git' with your Git repository URL
                git branch: 'main', url: 'git@github.com:DEL-ORG/s7michael-revive-ui.git'
            }
        }
    }
}

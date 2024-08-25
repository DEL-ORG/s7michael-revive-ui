pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sshagent(['github-ssh']) {
                    git branch: 'main', url: 'git@github.com:DEL-ORG/s7michael-revive-ui.git'
                }
            }
        }
    }
}

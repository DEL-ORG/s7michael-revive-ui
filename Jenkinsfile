pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                sshagent(['github-ssh']) {
                    git url: 'git@github.com:DEL-ORG/s7michael-revive-ui.git', branch: 'main'
                }
            }
        }
        // Other stages...
    }
}

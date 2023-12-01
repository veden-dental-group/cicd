pipeline {
    tools {
        nodejs "node 18"
    }
    agent {label 'test'}
    environment {
        GH_TOKEN = credentials('dev-github-token')
    }
    stages {
        stage('pull source code') {
            steps {
                git env.BRANCH_NAME: 'beta', credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental/cicd.git'
            }
        }
        
        stage('update version') {
            steps {
                sh "npm install"
                sh "npx semantic-release"
            }
        }
    }
}
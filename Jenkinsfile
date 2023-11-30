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
                script {
                    BRANCH = sh(script: "git rev-parse --abbrev-ref HEAD", returnStdout: true).trim()
                    echo "Current branch: ${BRANCH}"

                    if (BRANCH == 'main') {
                        git branch: 'main', credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental/cicd.git'
                    } else if (BRANCH == 'beta') {
                        git branch: 'beta', credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental/cicd.git'
                    } else {
                        error "Unsupported branch: ${BRANCH}"
                    }
                }
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
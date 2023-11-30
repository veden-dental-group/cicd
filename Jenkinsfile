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
                    def targetBranch = env.BRANCH_NAME
                    echo "Current branch: ${targetBranch}"

                    if (targetBranch == 'main') {
                        git branch: 'main', credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental/cicd.git'
                    } else if (targetBranch == 'beta') {
                        git branch: 'beta', credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental/cicd.git'
                    } else {
                        error "Unsupported branch: ${targetBranch}"
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
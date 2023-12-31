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
                git branch: env.BRANCH_NAME, credentialsId: 'github', poll: false, url: 'git@github.com:veden-dental-group/cicd.git'
            }
        }

        stage('check version') {
          steps {
              script {
                  VERSION = sh(script: "npm run version --silent", returnStdout: true).trim()
              }
          }
        }
        
        stage('update version') {
            steps {
                sh "npm install"
                sh "npx semantic-release"
            }
        }

        stage('check current version') {
          steps {
              script {
                  CURRENT_VERSION = sh(script: "npm run version --silent", returnStdout: true).trim()
              }
          }
        }

        stage('Build') {
            when {
                expression { CURRENT_VERSION == VERSION }
            }
            steps {
                echo "Build"
            }
        }

        stage('Deploy') {
            when {
                expression { CURRENT_VERSION == VERSION }
            }
            steps {
                echo "Deploy"
            }
        }
    }
}
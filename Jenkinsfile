pipeline {
    agent any
    
    environment {
        SLACK_CHANNEL = 'ip1'
        SLACK_CREDENTIALS = 'L4yQvfek4dHIxBPQhCxDBFtA'
    }
    
    tools {
        nodejs 'NodeJS 18.19.1'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        git url: 'https://github.com/WallaceJames/gallery.git', credentialsId: 'your-git-credentials-id'
                    } catch (err) {
                        echo "Error during git clone: ${err}"
                        error "Git clone failed"
                    }
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        sh 'npm install'
                    } catch (err) {
                        echo "Error during npm install: ${err}"
                        error "NPM install failed"
                    }
                }
            }
        }
        
        stage('Build the project') {
            steps {
                script {
                    try {
                        sh 'npm run build'
                    } catch (err) {
                        echo "Error during build: ${err}"
                        error "Build failed"
                    }
                }
            }
        }
        
        stage('Tests') {
            steps {
                script {
                    try {
                        sh 'npm test'
                    } catch (err) {
                        echo "Error during tests: ${err}"
                        error "Tests failed"
                    }
                }
            }
        }
    }
    
    post {
        always {
            slackSend (
                channel: env.SLACK_CHANNEL, 
                color: '#FFFF00', 
                message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) finished with status: ${currentBuild.currentResult}"
            )
        }
        success {
            slackSend (
                channel: env.SLACK_CHANNEL, 
                color: 'good', 
                message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) succeeded"
            )
        }
        failure {
            slackSend (
                channel: env.SLACK_CHANNEL, 
                color: 'danger', 
                message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) failed"
            )
        }
    }
}
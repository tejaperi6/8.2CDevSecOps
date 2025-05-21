pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}"
        RECIPIENTS = 'saiteja.phani@gmail.com'  // Replace with your email address
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/tejaperi6/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                always {
                    emailext attachLog: true,
                             subject: "Test Stage - Build ${currentBuild.fullDisplayName} - Status: ${currentBuild.currentResult}",
                             body: """The Test stage has completed with status: ${currentBuild.currentResult}.
Please check the attached console log for details.""",
                             to: "${RECIPIENTS}"
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext attachLog: true,
                             subject: "NPM Audit Stage - Build ${currentBuild.fullDisplayName} - Status: ${currentBuild.currentResult}",
                             body: """The NPM Audit stage has completed with status: ${currentBuild.currentResult}.
Please check the attached console log for details.""",
                             to: "${RECIPIENTS}"
                }
            }
        }
    }
}

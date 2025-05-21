pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}"
        RECIPIENTS = 'saiteja.phani@gmail.com'  // replace with your email
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
                             subject: "Test Stage - Build ${currentBuild.fullDisplayName}",
                             body: "The Test stage has completed. Please check the attached log.",
                             recipientProviders: [[$class: 'DevelopersRecipientProvider']],
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
                             subject: "NPM Audit Stage - Build ${currentBuild.fullDisplayName}",
                             body: "The NPM Audit stage has completed. Please check the attached log.",
                             recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                             to: "${RECIPIENTS}"
                }
            }
        }
    }
}

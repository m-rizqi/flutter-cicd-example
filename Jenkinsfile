pipeline {
    agent any

    environment {
        PATH = "/home/azureuser/flutter/bin/flutter"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Lint') {
            steps {
                sh 'flutter analyze'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'flutter test'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'flutter drive --target=test_driver/app.dart'
            }
        }

        stage('Build APK') {
            steps {
                sh 'flutter build apk'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/build/app/outputs/flutter-apk/app-release.apk', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        failure {
            emailext body: 'Something went wrong with the build.',
                recipientProviders: [culprits(), developers()],
                subject: 'Flutter Build Failed',
                to: 'muhammad.rizqi@divistant.com'
        }
    }
}

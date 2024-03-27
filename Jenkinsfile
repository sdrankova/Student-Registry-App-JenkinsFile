pipeline {
    agent any

    stages {
        stage('NPM Install') {
            steps {
                bat 'npm install'
            }
        }
        stage('NPM Audit') {
            steps {
                script {
                    def auditResult = bat(script: 'npm audit --json', returnStdout: true).trim()
                    if (!auditResult.contains("low") || !auditResult.contains("moderate") || !auditResult.contains("high")) {
                        echo "Vulnerabilities found. Fixing..."
                        bat "npm audit fix"
                    } else {
                        echo "No vulnerabilities found."
                    }
                }
            }
        }
        stage('Run Integration tests') {
            steps {
                bat 'npm run test'
            }
        }
        // stage('Build Image') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: '168cbf38-f38b-4613-a2a1-10f7e802a923', passwordVariable: 'pass', usernameVariable: 'user')]) {
        //             bat """docker build -t stefid/studentapp:1.0.0 .
        //                     docker login -u %user% --password %pass%
        //                     docker push stefid/studentapp:1.0.0"""
        //         }
        //     }
        // }
        stage('Deploy to production') {
            steps {
                script {
                    input("Deploy to production?")
                }
                echo 'Deploying...'
            }
        }
    }
}   
pipeline {
    agent any

    stages {
        stage('Build Client') {
            steps {
                script {
                    dir('client') {
                        sh 'docker build -t client-app .'
                    }
                }
            }
        }
        stage('Build Server') {
            steps {
                script {
                    dir('server') {
                        sh 'docker build -t server-app .'
                    }
                }
            }
        }
        stage('Deploy to Testing') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose.test.yml up -d'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Add your testing commands here
                    sh 'echo "Running tests..."'
                }
            }
        }
        stage('Deploy to Production') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    sh 'docker-compose -f docker-compose.prod.yml up -d'
                }
            }
        }
    }

    post {
        failure {
            mail to: 'team@example.com',
                 subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.JOB_NAME}."
        }
    }
}

pipeline {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '10', numToKeepStr: '3')
    }
    environment {
        DockerCred = credentials('Docker-cred')
    }
    stages {
        stage('Checkout stage') {
            steps {
                checkout scmGit(branches: [[name: '*/Test_Branch']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Aditya241193/django-todo-cicd.git']])
                
            }
        }
        stage('Docker-build') {
            steps {
                sh 'docker build -t adityasirothia/django-todo-app:$BUILD_NUMBER .'
            }
        }
        stage('Docker Push') {
            steps {
                sh 'echo $DockerCred_PSW | docker login -u $DockerCred_USR --password-stdin'
                sh 'docker push adityasirothia/django-todo-app:$BUILD_NUMBER'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
    post {
        always {
            emailext body: '''Hi there,

            I wanted to inform you that the Jenkins job "${JOB_NAME}" has completed successfully. Here are some details about the build:

            - Job Name: ${JOB_NAME}
            - Build Number: ${BUILD_NUMBER}
            - Build Status: ${BUILD_STATUS}
            - Build URL: ${BUILD_URL}

            Feel free to check the build logs and details on the Jenkins dashboard.

            Best regards,
            [Your Name]
            ''', subject: 'Subject: Jenkins Job Completion Notification - ${JOB_NAME} Build #${BUILD_NUMBER}', to: 'sirothia.aditya93@gmail.com'
        }
        success {
            echo "Job is Successful"
        }
        failure {
            echo "Job failed........."
        }
    }
}

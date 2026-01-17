pipeline {
 agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/Amankumar81/crypto-trade-template-51066-docker-jenkins.git'
            }
        }

        stage('Build and Push Images') {
            steps {
                script {
                    sh 'docker build -t amankumar81/crypto-images-tem:latest .'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'ay_pass', usernameVariable: 'ay_user')]) {
                        sh 'docker login -u $ay_user -p $ay_pass'
                        sh 'docker push amankumar81/crypto-images-tem:latest '
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker rm -f  crypto-container'
                    sh 'docker run -d --name crypto-container -p 8002:80 amankumar81/crypto-images-tem'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:8002'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/oshadapramod/GitHub-Docker-and-Jenkins-CI-CD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t oshadapramod/nodeapp-cuban:$BUILD_NUMBER .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test_dockerhub')]) {
                    script {
                        sh "docker login -u oshadapramod -p ${test_dockerhub}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push oshadapramod/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}

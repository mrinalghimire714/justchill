
pipeline {
    agent { label 'built-in'}
 
    environment {
        IMAGE_NAME = "mrinalghimire714/justchill"
        VERSION = "v{env.BUILD_NUMBER}"
        }
    
    stages{
        stage('Checkout') {
            steps {
                git (
                    url: 'https://github.com/mrinalghimire714/justchill.git'
                    branch: 'main',
                    credentialsId: '51334830-6e38-4cc0-9762-4cf253d421c4'
                )
            }
        }
 
        stage('Install and Test') {
            steps {
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r backend/requirements.txt
                    pytest backend/test_app.py
                '''
            }
            post {
                always {
                    junit 'results.xml'
                }
            }
        }
 
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$VERSION .'
                sh 'docker tag $IMAGE_NAME:$VERSION $IMAGE_NAME:latest'
            }
        }
 
        stage('Push to dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'a3af34f5-d317-453c-8af4 a90021 14c01b', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$VERSION'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
 
 
    }
}
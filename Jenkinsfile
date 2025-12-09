pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub')
        IMAGE = "seuusuario/meu-node-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/seuuser/seurepo.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker') {
            steps {
                sh "docker build -t $IMAGE:latest ."
            }
        }

        stage('Docker Login') {
            steps {
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push $IMAGE:latest"
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker stop meu-node-app || true
                docker rm meu-node-app || true
                docker run -d --name meu-node-app -p 80:3000 $IMAGE:latest
                """
            }
        }
    }
}

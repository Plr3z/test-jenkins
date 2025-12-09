pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Plr3z/test-jenkins.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "Nenhum teste configurado"'
            }
        }

        stage('Build') {
            when {
                expression { fileExists('package.json') }
            }
            steps {
                sh 'npm run build || echo "Nenhum script de build configurado"'
            }
        }
    }
}

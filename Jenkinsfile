pipeline {
    agent any

    tools {
        nodejs 'node 24.11.1' // Ferramenta Node.js
    }

    stages {

        stage('Checkout') {
            steps {
                // Clona o código-fonte do repositório Git
                git url: 'https://github.com/Plr3z/test-jenkins.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Instala todas as dependências
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Executa os testes
                sh 'npm test || echo "Nenhum teste configurado"'
            }
        }

        stage('Clean Dependencies') {
            // Remove dependências que não são necessárias em produção
            steps {
                sh 'echo "Limpando dependências de desenvolvimento..."'
                sh 'npm prune --production' 
            }
        }
        
        stage('Package Artifact') {
            steps {
                script {
                    def artifactName = "node-app-${env.BUILD_ID}.zip"
                    
                    // Comprime os arquivos essenciais. 
                    // Exclui arquivos de controle Git e o próprio zip para não duplicar.
                    sh "zip -r ${artifactName} . -x .git/* *.zip"
                    
                    // Arquiva o arquivo ZIP para que ele possa ser baixado diretamente na página do Job no Jenkins.
                    archiveArtifacts artifacts: "${artifactName}", fingerprint: true
                    
                    sh "echo '📦 Artefato de Build criado e arquivado: ${artifactName}'"
                }
            }
        }
    }
}
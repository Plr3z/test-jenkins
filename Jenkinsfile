pipeline {
    agent any

    tools {
        [cite_start]nodejs 'node 24.11.1' // Garante que a ferramenta Node.js esteja configurada [cite: 1]
    }

    stages {

        stage('Checkout') {
            steps {
                [cite_start]// Clona o código-fonte do repositório Git [cite: 1]
                git url: 'https://github.com/Plr3z/test-jenkins.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                [cite_start]// Instala todas as dependências [cite: 2]
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                [cite_start]// Executa os testes (ou a mensagem de teste padrão) [cite: 3]
                sh 'npm test || echo "Nenhum teste configurado"'
            }
        }

        stage('Clean Build') {
            // Este stage prepara o ambiente para o empacotamento final
            steps {
                sh 'echo "Limpando dependências de desenvolvimento para o artefato..."'
                // Remove dependências de desenvolvimento (se houver, com o package.json atual, não fará muita diferença)
                sh 'npm prune --production' 
            }
        }
        
        // NOVO STAGE: Criação e Arquivamento do Artefato
        stage('Package Artifact') {
            steps {
                script {
                    def artifactName = "node-app-${env.BUILD_ID}.zip"
                    
                    // Comprime os arquivos essenciais (código-fonte e dependências de produção)
                    // Excluímos node_modules, pois ele contém dependências dev que 'npm prune' não removeu.
                    // ATENÇÃO: Se o 'npm prune --production' falhar ou for insuficiente, você pode ter que incluir 'node_modules' no zip.
                    sh "zip -r ${artifactName} . -x node_modules/*"
                    
                    // Arquiva o arquivo ZIP para que ele possa ser baixado na página do Jenkins
                    archiveArtifacts artifacts: "${artifactName}", fingerprint: true
                    
                    sh "echo '📦 Artefato de Build criado e arquivado: ${artifactName}'"
                }
            }
        }
    }
}
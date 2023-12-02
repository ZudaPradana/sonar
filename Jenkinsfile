pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ZudaPradana/sonar']])
                echo 'Git Checkout Completed'
            }
        }

        stage('Run Spring Report') {
            steps {
                script {
                    // Ganti perintah berikut dengan yang sesuai untuk menjalankan laporan Spring
                    bat 'start /B mvnw.cmd spring-boot:run'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Tambahkan penundaan untuk memberikan waktu kepada server Spring Boot
                sleep time: 1, unit: 'MINUTES'
                withSonarQubeEnv('sq1') {
                    sh '''mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-analysis -Dsonar.projectName='sonar-analysis' -Dsonar.host.url=http://localhost:9000'''
                    echo 'SonarQube Analysis Completed'
                }
            }
        }
    }

    post {
        always {
            script {
                // Pastikan Anda telah mengkonfigurasi 'Telegram Bot Token' dan 'Chat ID' di Jenkins System Configuration
                telegramSend(message: "Pipeline finished: ${currentBuild.result}", chatId: '725260461')
            }
        }
    }


}

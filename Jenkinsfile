pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        IMAGE_NAME = "ymg-vize"
        CONTAINER_NAME = "nginx-vize-container"
    }

    stages {
        stage('Repoyu klonla') {
            steps {
                git url: 'https://github.com/DeryaKesgin/ymg2_vize.git', branch: 'main'
            }
        }

        stage('Docker image oluştur') {
            steps {
                echo "Docker image oluşturuluyor"
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Eski konteynerı durdur') {
            steps {
                echo "Eski konteyner durduruluyor (eğer varsa)"
                bat 'docker rm -f %CONTAINER_NAME% || exit 0'
            }
        }

        stage('Yeni konteyner oluştur') {
            steps {
                echo "Yeni konteyner başlatılıyor"
                bat 'docker run -d --name %CONTAINER_NAME% -p 4444:80 %IMAGE_NAME%'
            }
        }
    }

    post {
        success {
            echo "Yayın başarılı: http://localhost:4444"
        }
        failure {
            echo "Yayın başarısız oldu, lütfen logları kontrol edin."
        }
    }
}

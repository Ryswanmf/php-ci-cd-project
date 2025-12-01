pipeline {
    agent any

    environment {
        PHP_PATH = "D:\\laragon\\bin\\php\\php-8.1.10-Win32-vs16-x64\\php.exe"
        WORKSPACE_DIR = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Riswan-PHP-CI-CD-Pipeline"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat "\"%PHP_PATH%\" %WORKSPACE_DIR%\\composer.phar install"
            }
        }

        stage('Run Tests') {
            steps {
                bat "\"%PHP_PATH%\" vendor\\bin\\phpunit --testdox"
            }
        }

        stage('Code Quality - PHPStan') {
            steps {
                script {
                    def srcExists = fileExists('src')
                    if (srcExists) {
                        bat "\"%PHP_PATH%\" vendor\\bin\\phpstan analyse --level=max src"
                    } else {
                        echo "⚠️ Folder 'src' tidak ditemukan"
                        echo "PHPStan analysis di-skip"
                        echo "Untuk menggunakan PHPStan, buat folder 'src' dan pindahkan kode PHP ke sana"
                    }
                }
            }
        }

        stage('Build Artifact') {
            steps {
                echo "Building artifact..."
                // ZIP semua isi project
                bat 'tar -a -c -f build.zip .'
            }
        }

        stage('Deploy to FTP') {
            steps {
                echo "Deploying to FTP server..."
                ftpPublisher alwaysPublishFromMaster: true,
                publishers: [
                    [
                        configName: 'FTP-SERVER',
                        transfers: [
                            [
                                sourceFiles: 'build.zip',
                                remoteDirectory: '/public_html/ci-cd-project',
                                removePrefix: '',
                                flatten: false
                            ]
                        ]
                    ]
                ]
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline selesai dan berhasil!"
        }
        failure {
            echo "❌ Pipeline gagal! Cek log error di atas."
        }
    }
}
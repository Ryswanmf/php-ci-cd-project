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
                bat 'tar -a -c -f build.zip .'
                echo "✅ Artifact build.zip berhasil dibuat"
            }
        }

        stage('Deploy to FTP') {
            steps {
                echo "📦 Build artifact: build.zip"
                echo "✅ Artifact siap untuk di-deploy"
                echo "⚠️ FTP deployment belum dikonfigurasi"
                echo ""
                echo "Untuk mengaktifkan FTP deployment:"
                echo "1. Install plugin 'Publish Over FTP' di Jenkins"
                echo "2. Konfigurasi FTP server di Manage Jenkins > System"
                echo "3. Update Jenkinsfile dengan konfigurasi FTP"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline selesai dan berhasil!"
            echo "📊 Summary:"
            echo "   - Dependencies: Installed"
            echo "   - Tests: Passed"
            echo "   - Build: Success"
            echo "   - Artifact: build.zip"
        }
        failure {
            echo "❌ Pipeline gagal! Cek log error di atas."
        }
    }
}
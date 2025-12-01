pipeline {
    agent any

    environment {
        PHP_PATH = "C:\\laragon\\bin\\php\\php-8.1.10\\php.exe"
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
                bat "\"%PHP_PATH%\" vendor\\bin\\phpstan analyse --level=max src"
            }
        }

        stage('Build Artifact') {
            steps {
                // ZIP semua isi project
                bat 'tar -a -c -f build.zip .'
            }
        }

        stage('Deploy to FTP') {
            steps {
                ftpPublisher alwaysPublishFromMaster: true,
                publishers: [
                    ftpPublisherDesc(
                        configName: 'FTP-SERVER',
                        transfers: [
                            ftpTransfer(
                                sourceFiles: 'build.zip',
                                remoteDirectory: '/public_html/ci-cd-project',
                                removePrefix: '',
                                flatten: false
                            )
                        ]
                    )
                ]
            }
        }
    }

    post {
        success {
            echo "Pipeline selesai dan berhasil!"
        }
        failure {
            echo "Pipeline gagal! Cek log error di atas."
        }
    }
}

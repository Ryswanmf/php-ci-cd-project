pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"C:\\laragon\\bin\\php\\php-8.1.10\\php.exe" C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Riswan-PHP-CI-CD-Pipeline\\composer.phar install'
            }
        }

        stage('Run Tests') {
            steps {
                bat '"C:\\laragon\\bin\\php\\php-8.1.10\\php.exe" vendor\\bin\\phpunit --testdox'
            }
        }
    }
}

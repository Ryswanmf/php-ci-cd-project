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
                bat 'php composer.phar install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'php vendor\\bin\\phpunit --testdox'
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/syifaamdh/sast-demo-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install bandit'
            }
        }

        stage('SAST Analysis') {
            steps {
                sh 'bandit -f xml -o bandit-output.xml -r .'
            }
        }
    }
}

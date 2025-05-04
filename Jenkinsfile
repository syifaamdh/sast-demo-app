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
                sh 'bandit -r app.py -f xml -o bandit.xml || true'
                // Cek apakah bandit.xml ada
                sh 'cat bandit.xml'
            }
        }

        stage('Post Build') {
            steps {
                script {
                    // Pastikan Jenkins membaca file bandit.xml setelah pipeline selesai
                    if (fileExists('bandit.xml')) {
                        // Ini untuk menggunakan plugin Warnings Next Generation
                        recordIssues(
                            tools: [bandit(pattern: 'bandit.xml')],
                            enabledForFailure: true
                        )
                    }
                }
            }
        }
    }
}

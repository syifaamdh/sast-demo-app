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
                // Jalankan Bandit dan simpan hasilnya dalam format XML
                sh 'bandit -r . -f xml -o bandit.xml'

                // Tampilkan isi file sebagai debug (opsional)
                sh 'cat bandit.xml'
            }
        }
    }

    post {
        always {
            // Konfigurasi agar Jenkins membaca hasil dari bandit.xml
            recordIssues(tools: [bandit(pattern: 'bandit.xml')])
        }
    }
}

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
                // Menjalankan Bandit untuk melakukan static analysis pada file Python (app.py)
                // Output dalam format XML ke file bandit.xml
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
                            tools: [bandit(pattern: 'bandit.xml')]
                        )
                    } else {
                        echo 'File bandit.xml tidak ditemukan.'
                    }
                }
            }
            post {
                always {
                    // Mengarsipkan hasil bandit.xml
                    archiveArtifacts artifacts: 'bandit.xml', allowEmptyArchive: true
                    // Menambahkan laporan Bandit dalam Jenkins jika diperlukan
                    junit 'bandit.xml'  // Jika format laporan sesuai dengan format JUnit XML
                }
            }
        }
    }
}

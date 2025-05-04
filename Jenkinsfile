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
                // Jalankan analisis Bandit dan simpan hasilnya ke bandit.xml
                sh 'bandit -r app.py -f xml -o bandit.xml || true'
                sh 'cat bandit.xml'  // Menampilkan isi bandit.xml di Jenkins console
            }
        }

        stage('Post Build') {
            steps {
                script {
                    if (fileExists('bandit.xml')) {
                        echo "File bandit.xml ditemukan. Menampilkan isi file:"
                        sh 'cat bandit.xml'
                        
                        // Menampilkan issues menggunakan Warnings Next Generation
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
                }
            }
        }
    }
}

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
                sh 'cat bandit.xml'
            }
        }

        stage('Post Build') {
            steps {
                script {
                    if (fileExists('bandit.xml')) {
                        echo "File bandit.xml ditemukan. Menampilkan isi file:"
                        sh 'cat bandit.xml'
                        
                        // Menggunakan plugin Warnings Next Generation untuk mengolah hasil analisis Bandit
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
                    archiveArtifacts artifacts: 'bandit.xml', allowEmptyArchive: true
                }
            }
        }
    }
}

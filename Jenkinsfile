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
                sh 'pip install bandit'
            }
        }

        stage('SAST Analysis') {
            steps {
                sh 'bandit -r app.py -f xml -o bandit.xml'
            }
        }

        stage('Post Build') {
            steps {
                script {
                    if (fileExists('bandit.xml')) {
                        echo 'File bandit.xml found. Displaying contents:'
                        sh 'cat bandit.xml'
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts 'bandit.xml'
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/syifaamdh/sast-demo-app.git'
            }
        }
        stage('Static Analysis with Bandit') {
            steps {
                script {
                    sh 'bandit -r app.py -f xml -o bandit.xml'
                }
            }
        }
        stage('Publish Bandit Results') {
            steps {
                // Menampilkan hasil analisis
                recordIssues(
                    tools: [bandit(pattern: 'bandit.xml')],
                    enableForFailure: true
                )
            }
        }
    }
}

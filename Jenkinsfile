pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-pat', url: 'https://github.com/AnseqPL/abcd-student.git', branch: 'main'
                }
            }
        }
        stage('TruffleHog Scan on GitHub Repo') {
            steps {
                script {
                    sh '''
                        docker run --rm trufflesecurity/trufflehog:latest git https://github.com/AnseqPL/abcd-student.git > /tmp/trufflehog_report.json
                    '''
                }
            }
        }
    }
    post {
            always {
                defectDojoPublisher(artifact: '/tmp/trufflehog_report.json', 
                    productName: 'Juice Shop', 
                    scanType: 'Trufflehog Scan', 
                    engagementName: 'adam.natonik@gmail.com')
            }
        }
}
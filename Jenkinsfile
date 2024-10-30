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
                    // Pobranie kodu z GitHub
                    git credentialsId: 'github-pat', url: 'https://github.com/AnseqPL/abcd-student.git', branch: 'main'
                }
            }
        }
        stage('Semgrep Scan') {
            steps {
                script {
                    sh '''
                        # Uruchomienie skanowania Semgrep na pobranym kodzie
                        semgrep --config=auto . --json --json-output=/tmp/semgrep.json
                    '''
                }
            }
        }
    }
    post {
            always {
                defectDojoPublisher(artifact: '/tmp/semgrep.json', 
                    productName: 'Juice Shop', 
                    scanType: 'Semgrep JSON Report', 
                    engagementName: 'adam.natonik@gmail.com')
            }
        }
}
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
        stage('TruffleHog') {
            steps {
                script {
                    // Uruchamiamy Trufflehog w kontenerze Docker
                    sh '''
                        docker run --rm -v "$PWD:/repo" trufflesecurity/trufflehog:latest git file:///repo > trufflehog_report.json
                    '''
                }
            }
        }
    }
    // post {
    //         always {
    //             defectDojoPublisher(artifact: '/tmp/sca-osv-scanner.json', 
    //                 productName: 'Juice Shop', 
    //                 scanType: 'Trufflehog Scan', 
    //                 engagementName: 'adam.natonik@gmail.com')
    //         }
    //     }
}
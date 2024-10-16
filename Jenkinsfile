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
        stage('SCA') {
            steps {
                // sh '''
                //     docker run --name juice-shop -d --rm -p 3000:3000 bkimminich/juice-shop
                //     sleep 5
                // '''
                sh '''
                    ls -laht /tmp/
                '''
                // sh '''
                //     osv-scanner scan --lockfile package-lock.json --format json --output /tmp/sca-osv-scanner.json
                // '''
                // sh '''
                //     cat /tmp/sca-osv-scanner.json
                // '''
            }
        }
    }
    post {
            always {
                defectDojoPublisher(artifact: '/tmp/sca-osv-scanner.json', 
                    productName: 'Juice Shop', 
                    scanType: 'OSV Scan', 
                    engagementName: 'adam.natonik@gmail.com')
            }
        }
}
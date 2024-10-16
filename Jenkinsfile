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
                sh '''
                    docker run --name juice-shop -d --rm -p 3000:3000 bkimminich/juice-shop
                    sleep 5
                '''
                sh '''
                    rm /tmp/sca-osv-scanner.json
                '''
                sh '''
                    osv-scanner scan --lockfile package-lock.json --format json --output /tmp/sca-osv-scanner.json
                '''
                sh '''
                    cat /tmp/sca-osv-scanner.json
                '''
            }
        }
    }
}
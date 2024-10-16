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
        stage('Run JShop') {
            steps {
                sh '''
                    docker run --name juice-shop -d --rm \\
                        -p 3000:3000 \\
                        bkimminich/juice-shop\\
                        -v 
                    sleep 5
                '''
                
                sh '''
                    osv-scanner scan --lockfile package-lock.json --format json --output sca-osv-scanner.json
                '''
                sh '''
                    cat sca-osv-scanner.json
                '''
            }
        }
    }
}

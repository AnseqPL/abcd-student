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
                        bkimminich/juice-shop
                    sleep 5
                '''
<<<<<<< HEAD
                
                sh '''
                    osv-scanner scan --lockfile package-lock.json --format json --output results/sca-osv-scanner.json
=======
                 sh '''
                    docker run --name zap --rm \
                    --add-host=host.docker.internal:host-gateway \
                    -v /home/adam/DevSecOps/abcd-student/.zap/passive.yaml:/zap/wrk/passive_scan.yaml:rw \
                    -v /home/adam/DevSecOps/abcd-student/.zap/reports:/zap/wrk:rw \
                    -t ghcr.io/zaproxy/zaproxy:stable bash -c \
                    "zap.sh -cmd -addonupdate && \
                    zap.sh -cmd -addoninstall communityScripts && \
                    zap.sh -cmd -addoninstall pscanrulesAlpha && \
                    zap.sh -cmd -addoninstall pscanrulesBeta && \
                    zap.sh -cmd -autorun /zap/wrk/passive_scan.yaml"
>>>>>>> 3285e6badfffc7eff9fa8d2589057f43c9422645
                '''
            }
        }
    }
}

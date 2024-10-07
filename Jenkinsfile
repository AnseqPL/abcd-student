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
        stage('Example') {
            steps {
                echo 'Hello!'
                sh 'ls -la'
            }
        }
        stage('Check Git') {
            steps {
                echo 'GitUpdated!'
            }
        }
        stage('[ZAP] Baseline passive-scan') {
            steps {
                sh '''
                    docker run --name juice-shop -d --rm \\
                        -p 3000:3000 \\
                        bkimminich/juice-shop
                    sleep 5
                '''
                 sh '''
                    docker run -u zap --rm \\
                        --add-host=host.docker.internal:host-gateway \\
                        -v ./passive_scan.yaml:/zap/wrk/:rw
                        -t ghcr.io/zaproxy/zaproxy:stable bash -c \\
                        "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive_scan.yaml" \\
                        || true
                '''
                sh '''
                    docker kill juice-shop
                    sleep 5
                '''
            }
        }
    }
}

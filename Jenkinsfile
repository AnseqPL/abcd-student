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
        stage('[ZAP] Baseline passive-scan') {
            steps {
                sh '''
                    docker run --name juice-shop -d --rm \\
                        -p 3000:3000 \\
                        bkimminich/juice-shop
                    sleep 5
                '''
                 sh '''
                    docker run --name zap --rm \
                    --add-host=host.docker.internal:host-gateway \
                    -v /home/adam/DevSecOps/abcd-student/.zap/passive.yaml:/zap/wrk/passive_scan.yaml:rw \
                    -v /home/adam/Downloads/reports/:/zap/wrk/reports \
                    -t ghcr.io/zaproxy/zaproxy:stable bash -c \
                    "zap.sh -cmd -addonupdate && \
                    zap.sh -cmd -addoninstall communityScripts && \
                    zap.sh -cmd -addoninstall pscanrulesAlpha && \
                    zap.sh -cmd -addoninstall pscanrulesBeta && \
                    zap.sh -cmd -autorun /zap/wrk/passive_scan.yaml"
                '''
            }
        }
    }
    post {
        always {
                defectDojoPublisher(artifact: '/zap/wrk/reports/zap_xml_report.xml', 
                    productName: 'Juice Shop', 
                    scanType: 'ZAP Scan', 
                    engagementName: 'adam.natonik@gmail.com')
            }
        always {
            sh '''
                docker stop juice-shop
            '''
        }    
    }
}

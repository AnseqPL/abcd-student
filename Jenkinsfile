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
        stage('Semgrep Scan') {
            steps {
                script {
                    sh '''
                        docker run --rm -v $PWD:/src semgrep/semgrep:latest semgrep --config auto /src
                    '''
                }
            }
        }
    }
}
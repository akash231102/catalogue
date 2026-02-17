pipeline {
    agent {
        node {
            label "agent"
        }
    }
    environment{
        course = "jenkins"
        defAppversion = ""
    }
    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    // this is build section
    stages{
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    Appversion = packageJson.version
                    echo "app version: ${Appversion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('BUild image') {
            steps {
                script{
                    sh """
                        docker build -t catalogue:${Appversion} .
                        docker images
                    """
                }
            }
        }
    }
}
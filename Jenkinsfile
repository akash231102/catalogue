pipeline {
    agent {
        node {
            label "agent"
        }
    }
    environment{
        course = "jenkins"
        defAppversion = ""
        acc_id = "382488149327"
        project = "roboshop"
        component = "catalogue"
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
        stage('testing') {
            steps {
                script{
                    sh """
                        npm test
                    """
                }
            }
        }
        stage('BUild image') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-auth') {
                        sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${acc_id}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${acc_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${Appversion} .
                        docker images
                        docker push ${acc_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${Appversion}
                        """
                    }
                }
            }
        }
    }
}
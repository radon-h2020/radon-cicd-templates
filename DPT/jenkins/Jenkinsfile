pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret')
    }
    stages {
        stage('Install dependacies') {
            steps {
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'echo install any dependancies..'
                }
            }
        }
        stage('Run DPT') {
            environment {
                DEPLOY_FILE = 'todolist-dev.csar'
                DPT_DOCKER_NAME = 'RadonDPT'
                DPT_FILES_PATH = '{"path":"/tmp/radon/container/main.cdl"}'
            }
            steps {
                sh 'echo Start VT container and perform verify test...'
                sh 'unzip -o ${DEPLOY_FILE}'
                sh 'mkdir -p $PWD/tmp/radon && cp -r todolist-dev.csar $PWD/tmp/radon'
                sh 'git clone https://github.com/radon-h2020/radon-defect-prediction-api'
                sh 'cd radon-defect-prediction-api && docker build -t radon-dp:latest .'
                sh 'docker run --name "${DPT_DOCKER_NAME}" --rm -d -p 5000:5000 -v $PWD/tmp/radon:/tmp/radon/container radon-dp:latest'
                sh 'sleep 5'
                sh 'docker ps'
                sh 'curl -X POST "http://localhost:5000/api/classification/classify" -H  "accept: */*" -H  "Content-Type: plain/text" -d "- host: all"'
                sh 'docker stop ${DPT_DOCKER_NAME}'
            }
        }
        stage('Test functionality') {
            environment {
                 finish = 'Done!'
            }
            steps {
                sh 'echo Testing functionality...'
                sh 'echo $finish'
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}


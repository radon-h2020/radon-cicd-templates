pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret')
    }
    stages {
        stage('Fetch and unpack template') {
            steps {
                sh "echo $csar_reference"
                sh "echo download"
            }
        }

        stage('Deploy resources') {
            environment {
                DEPLOY_FILE = '_definitions/radonlegacyblueprints__ImageResize.tosca'
            }
            steps {
                sh 'pip3 install boto3 ansible opera --user'
                sh 'mkdir -p /tmp && cp nodetypes/radon.nodes.legacy/AwsCreateRole/files/policy/policy.json /tmp/policy.json'
                sh 'PATH="$(python3 -m site --user-base)/bin:${PATH}" && opera deploy $DEPLOY_FILE'
            }
        }

        stage('Test functionality') {
            environment {
                IMAGE = 'setter.jpg'
            }
            steps {
                sh 'echo Install awscli'
                sh 'pip install awscli --user'

                sh 'echo Upload image'
                sh 'curl https://www.dogsnsw.org.au/media/img/BrowseAllBreed//Irish-Setter.jpg > $IMAGE'
                sh 'PATH="$(python3 -m site --user-base)/bin:${PATH}" && aws s3 mv $IMAGE s3://radon-particles'

                sh 'PATH="$(python3 -m site --user-base)/bin:${PATH}" && echo "$(aws s3 ls radon-particles-resized)"'
            }
        }
    }
}

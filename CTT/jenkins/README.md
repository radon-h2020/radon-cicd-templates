For further information about the use, see https://continuous-testing-tool.readthedocs.io/en/latest/

Note: This pipeline assumes that:
    1) The application deployable csar file, the testing infrastructure deployable csar file, the inputs.yaml file and the ctt_config.yaml file, all reside in the the directory radon-ctt-cli-testing of a remote repository.
    
    2) The inputs.yaml file has been configured with the VPC-network ID by the user.

    3) The AWS_SSH_KEY key has been created on AWS and stored on the Jenkins instance as secret file by the user.

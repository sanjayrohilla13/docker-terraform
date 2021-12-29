def STAGE_TO_RUN =params.STAGE
pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2'
        //TEMP_VAR = credentials('srv-ecr-usr')
        def DOCKER_SOURCE = " "
    }

    parameters{
        choice(name: 'ENVIRONMENT', choices: ['dev','prod'], description: 'Select the Environment Name')
        choice(name: 'DOCKER_SRC', choices: ['ECR','artifactory'], description: 'Source of Sonarqube Docker Source')
        choice(name: 'STAGE', choices: ['plan','deploy','destroy'], description: 'Terraform Stage to Apply')
        string(name: 'BUCKETNAME', defaultValue: 'sanjayrohilla', description: 'Enter the name of S3 Bucket')
        string(name: 'KEYNAME', defaultValue: 'terraform/tfstate.tfstate', description: 'Enter the object name in the S3 Bucket')
    }
    stages {
        stage('Download GIT Hub Repo') {
            steps {
                echo 'Downloading..'
                script {
                    git 'https://github.com/sanjayrohilla13/docker-terraform.git'
                }
            }
        }
// plan stage
        stage('Initialize') {
            when{expression{STAGE_TO_RUN.contains('init')}}
            steps {
                    sh '''
                        echo ${BUCKETNAME}
                        echo ${KEYNAME}
                    
							|| not_exist=true
							
                        if [ $not_exist ]; then
                            echo "Object doesn't exist"
                        else
                            echo "Object exists"
                        fi
                        '''  
            }  
        }
    }
}


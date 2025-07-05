pipeline {
    agent any

    environment {
        version="{0.1.${BUILD_NUMBER}"
    }

    stages {
        stage('code') {
            steps {
                checkout scm
            }
        }

        stage('Image build') {
            steps {
                sh 'docker build -t demon .'
            }
        }

        stage('git hub push') {
            steps {
                sh 'docker tag demon demonikk/breathefree:0.1.0'
            }
        }

        stage('image push') {
            steps {
                sh '''
                    docker login -u demonikk -p rickANDmorty
                    docker push demonikk/breathefree:0.1.${VERSION}
                '''
            }
        }

	stage('Prepare init.sh') {
            steps {
                sh """
                    sed 's/__Version__/${VERSION}/g' terraform/breathefree/init.sh > terraform/breathefree/init_temp.sh
                    mv terraform/breathefree/init_temp.sh terraform/breathefree/init.sh
                """
            }
	}

        stage('ssh login') {
	    steps {
                 sh '''
            	     ssh -o StrictHostKeyChecking=no -i /home/ubuntu/terra.pem ubuntu@3.106.129.114 '
           	     cd ~/terraform/breathefree &&
            	     terraform init &&
                     terraform apply -auto-approve
            	     '
        	     '''
    		}
	    }
	}
}

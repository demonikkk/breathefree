pipeline {
    agent any

    stages {
        stage('code') {
            steps {
                sh 'git clone https://github.com/demonikkk/breathefree.git'
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
                    docker push demonikk/breathefree:0.1.0
                '''
            }
        }

        stage('ssh login') {
	        steps {
                 sh '''
            	     ssh -o StrictHostKeyChecking=no -i /home/ubuntu/terra.pem ubuntu@3.27.213.172 << EOF
           	         cd ~/terraform/breathefree
            	     terraform init
                     terraform apply -auto-approve
            	     EOF
        	     '''
    		}
	    }
	}
}

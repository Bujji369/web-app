def eksCluster="my-eks-cluster"
def region="ap-south-1"

pipeline {
    agent any
	tools {
        maven "maven"
    }
    stages {
        stage('Git Clone') {
            steps {
			   script {
			   
               git 'https://github.com/Bujji369/web-app.git'
			   
			   }
            }
        }
        stage('Maven Build') {
            steps {
			   script {
			   
               sh 'mvn -B -DskipTests clean package'
			   
			   }
            }
        }
		stage('Docker image build') {
            steps {
			   script {
			   
               sh "docker version"
			   sh "cat Dockerfile"
               sh "docker build -t srirammani/sriram369:mani-ram ." 
               sh "docker images"
			   
			   }
            }
        }
		stage('image upload DockerHub') {
            steps {
			   script {
			   
                  withCredentials([string(credentialsId: 'DOCKER-HUB-CREDENTIALS', variable: 'DOCKERHUB')]) {
            
                    sh "docker login -u srirammani -p ${DOCKERHUB}"
                  }
          
                  sh "docker push srirammani/sriram369:mani-ram"
			   
			   }
            }
        }
		
		stage('EKS Deployment') {
            steps {
			  
		                                    
                    script{
					
                    	sh "kubectl apply -f maven-web-app-deploy.yml"
                        sh "sleep 20s"
					}
			  
			   
			   
            }
        }
	}
	
}

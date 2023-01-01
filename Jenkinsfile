def eksCluster="my-eks-cluster"
def region="ap-south-1"

pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
			   script {
			   
               git branch: 'main', url: 'https://github.com/Bujji369/k8s-jenkins-aws.git'
			   
			   }
            }
        }
        stage('Gradle Build') {
            steps {
			   script {
			   
               sh "./gradlew build"
			   
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
			  
			  
			    withCredentials([[
                      $class: 'AmazonWebServicesCredentialsBinding',
                      credentialsId: 'aws-credentials', 
                      accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                  
                                   
                   sh "aws eks update-kubeconfig --name $eksCluster --region $region"
                   
                   sh "echo $eksCulster       $region"
                    
                    script{
					
                    	sh "kubectl apply -f k8s-spring-boot-deployment.yml"
                        sh "sleep 20s"
					}
			  
			   
			   }
            }
        }
	}
	
}

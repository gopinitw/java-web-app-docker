pipeline {
    agent any
    environment{
            PATH="$PATH:/usr/share/apache-maven"
            registry = "904440666777.dkr.ecr.us-east-1.amazonaws.com/jenkins-pipeline-demo3"
    }
stages {
      stage ('Run entire Build'){
            
       stages{
       
        stage('MVN package Build') {
            steps {
                sh 'mvn clean install'
            }
        }
            stage('jacoco report coverage'){
            steps{
                jacoco()
                }
               }
           stage('Build Docker Image') {
              steps {
                  sh 'docker build -t $registry:dev .'            
              }
          }
          stage('Push to Amazon ECR'){
              steps {
                  sh 'docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 904440666777.dkr.ecr.us-east-1.amazonaws.com'
                  sh 'docker push $registry:dev'
                    }
            }
	        stage ('Deploy to EKS cluster') {
	             steps{
                        kubernetesDeploy(
                        configs: 'deployment.yaml',
                        kubeconfigId: 'k8s',
                        enableConfigSubstitution: true
                                        )  
                    }
                                            } 
       }
	     
        }
    }
}

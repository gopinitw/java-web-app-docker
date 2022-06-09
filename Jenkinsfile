pipeline {
    agent any
    environment{
            PATH="$PATH:/usr/share/apache-maven"
            registry = "904440666777.dkr.ecr.us-east-1.amazonaws.com/jenkins-pipeline-demo"
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
	    stage('SonarQube analysis') {
            steps {
              withSonarQubeEnv('SonarQube') {
                sh "sonarQube/bin/sonar-scanner \
                   -Dsonar.login=admin \
                   -Dsonar.password=Vijaya@172510 \
                   -Dsonar.projectKey=demoapp \
		           -Dsonar.projectName=fairidemo \
		           -Dsonar.projectVersion=1.0.0 \
		           -Dsonar.sources=src \
		           -Dsonar.java.binaries=target/classes \
		           -Dsonar.sourceEncoding=UTF-8 \
		           -Dsonar.language=java \
		           -Dsonar.junit.reportPaths=target/surefire-reports \
		           -Dsonar.surefire.reportsPath=target/surefire-reports \
		           -Dsonar.coverage.jacoco.xmlReportPaths=target/jacoco-ut/jacoco.xml \
		           -Dsonar.java.coveragePlugin=jacoco \
		           -Dsonar.exclusions=**src/test/**,**/com/snn/fai/model/**,**/com/snn/fai/model/json/**,**/com/snn/fai/sdk/**,**/com/snn/fai/solo/soap/wsdl/**,**/com/snn/fai/solo/soap/model/response/lic/**,**/com/snn/fai/solo/soap/model/request/registration/**,**/com/snn/fai/solo/soap/model/response/registration/**,**/org/snn/api/**,**com/snn/fai/feign/**,**/com/snn/fai/solo/soap/model/request/lic/**,**/com/snn/fai/solo/soap/model/response/lic/**,**com/snn/fai/jwt/**,**com/snn/fai/handler/exception/**" 
                      }
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
                        configs: 'deployment.yml',
                        kubeconfigId: 'k8s',
                        enableConfigSubstitution: true
                                        )  
                    }
                                            } 

       }
      }
}
}

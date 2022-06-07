pipeline {
    agent any
    environment{
            PATH="$PATH:/usr/share/apache-maven"
            scannerHome = tool 'SonarQube'
            registry = "491655376147.dkr.ecr.us-east-1.amazonaws.com/fairi-case-management"
            }
stages {
      stage ('Run entire Build'){
            
       stages{
       
        stage('MVN package Build') {
            steps {
                sh 'mvn clean install'
            }
        }
       }
      }
}
}

pipeline {
    agent any
    environment{
            PATH="$PATH:/usr/share/apache-maven"
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
       }
      }
}
}

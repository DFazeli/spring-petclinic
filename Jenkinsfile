pipeline {
    agent { label 'linux' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/effectivejenkins/spring-petclinic.git'
          }
        }
        stage('Build') {
          //  agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
              
            sh 'scp target/*.jar david@192.168.64.13:/home/david/spring-petclinic'
            sh """
                ssh david@192.168.64.13 'mkdir /home/david/spring-petclinic' 
                ssh david@192.168.64.13 'nohup java -jar /home/david/spring-petclinic-1.5.1.jar &'
               
               """  
          }
        }
    }
}

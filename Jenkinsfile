pipeline {
    agent any
    stages {
        stage('Maven Build') {
            steps {
                script {
                    def mavenHome = tool name: "Maven-3.8.6", type: "maven"
                    def mavenCMD = "${mavenHome}/bin/mvn"
                    sh "${mavenCMD} clean package"
                }
            }
        }
        stage('SonarQube analysis') {       
            steps {
                withSonarQubeEnv('Sonar-Server-7.8') {
                    sh "mvn sonar:sonar"    	
                }
            }
        }
        stage ( 'upload build artifact'){
      }
  }
}    

    

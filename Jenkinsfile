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
                withSonarQubeEnv('sonarscanner4') {
                    sh "mvn sonar:sonar"    	
                }
            }
        }
    }
}


    

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
                script {
                    withSonarQubeEnv('Sonar-Server-7.8') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage('Upload Build Artifact') {
            steps {
                script {
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: '01-maven-web-app',
                            classifier: '',
                            file: 'target/01-maven-web-app.war',
                            type: 'war'
                        ]
                    ],
                    credentialsId: 'nexuslogin',
                    groupId: 'mynexusproject',
                    nexusUrl: 'http://3.25.92.209:8081', 
                    protocol: 'http',
                    repository: 'vprofile-snapshot',
                    version: '1.0.0'
                }
            }
        }
    }
}

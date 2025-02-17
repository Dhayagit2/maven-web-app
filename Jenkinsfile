pipeline {
    agent any
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "3.25.92.209:8081"
        NEXUS_REPOSITORY = "vprofile-release"
        NEXUS_REPO_ID = "vprofile-release"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
    }
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
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: env.NEXUS_URL,
                        groupId: 'com.example',
                        version: env.ARTVERSION,
                        repository: env.NEXUS_REPOSITORY,
                        credentialsId: env.NEXUS_CREDENTIAL_ID,
                        artifacts: [
                            [artifactId: 'myproject',
                             classifier: '',
                             file: "target/01-maven-web-app.war",
                             type: 'war']
                        ]
                    )
                }
            }
        }
        stage('Deploy') { // Added missing stage name 'Deploy'
            steps {
                script {
                    sshagent(['sshdeploy']) {
                        sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ubuntu@54.206.150.178:/home/ubuntu/apache-tomcat-9.0.65/webapps'
                    }
                }
            }
        }
    }
}



node {
    stage('Clone repo') {
        git credentialsId: 'GIT-Credentials', url: 'https://github.com/ashokitschool/maven-web-app.git'
    }

    stage('SonarQube analysis') {       
        withSonarQubeEnv('sonar-server') {
            sh "mvn sonar:sonar"
        }
    }
}


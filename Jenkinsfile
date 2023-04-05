pipeline {
    agent none
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3.8.3-openjdk-17' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
            steps { 
               sh 'mvn clean install'  
            }
        }

        stage("SonarQube") {
            agent any
            steps {
                script{
                    scannerHome = tool 'SonarScanner';
                }
                withSonarQubeEnv('SonarScanner') {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -D sonar.login=admin \
                    -D sonar.password=password \
                    -D sonar.projectKey=spring-petclinic \
                    -D sonar.host.url=http://sonarqube:9000/ \
                    -D sonar.sources=src/main/java/ \
                    -D sonar.java.binaries=target/classes"
                }
            }
        }  
    }
}
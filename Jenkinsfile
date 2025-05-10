pipeline {
    agent any
    environment{
        SCANNER_HOME= tool 'sonar'
    }
    

    stages {
        stage('checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/NaruPraveenKumar/Vitual-Browser.git'
               }
        }
        stage('owsap') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableNodeAudit', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependen-check-report.xml'
            }
        }
         stage('build') {
            steps {
                withSonarQubeEnv('sonar') {
               sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=virtual -Dsonar.projectName=virtual"
          }
            }
        }
     
       stage('dcoker build') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub') {
                    dir('/var/lib/jenkins/workspace/virtual/.docker/google-chrome') {
                     sh 'docker build -t narupraveen/virtual:1.0.0 .'
           }
             }
                }
                }
               }
               stage('trivu check') {
            steps {
                sh'trivy image --format table -o image.html narupraveen/virtual:1.0.0'
            }
        }
         stage('dcoker push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub') {
                   
                     sh 'docker push  narupraveen/virtual:1.0.0 '
             }
                }
                }
               }
               stage('dcoker-compose') {
            steps {
               sh 'docker-compose up -d'
                }
                }
               
    }
}

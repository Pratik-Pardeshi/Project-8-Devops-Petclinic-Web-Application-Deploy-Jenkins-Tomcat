pipeline {
    agent any 
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    stages {
        
        stage("Git Checkout") {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Pratik-Pardeshi/Project-7-Devops-Petclinic-Web-Application-Deploy-Jenkins-Tomcat.git'
            }
        }
        
        stage("Compile") {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage("Test Cases") {
            steps {
                sh "mvn test"
            }
        }
        
        stage("Build") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage("Docker Build & Push") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t image1 ."
                        sh "docker tag image1 pratikp02/pet-clinic123:latest"
                        sh "docker push pratikp02/pet-clinic123:latest"
                    }
                }
            }
        }
        
        stage("TRIVY") {
            steps {
                sh "trivy image pratikp02i/pet-clinic123:latest"
            }
        }
        
        stage("Deploy To Tomcat") {
            steps {
                sh "cp /var/lib/jenkins/workspace/CI-CD/target/petclinic.war /opt/apache-tomcat-9.0.91/webapps/"
            }
        }
    }
}

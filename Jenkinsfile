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
                sh "mvn clean package"
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
        
        stage("Deploy To Tomcat") {
            steps {
                script {
                    def warFile = "/var/lib/jenkins/workspace/${env.JOB_NAME}/target/petclinic.war"
                    if (fileExists(warFile)) {
                        sh "cp ${warFile} /opt/apache-tomcat-9.0.93/webapps/"
                    } else {
                        error "WAR file not found at ${warFile}"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/aj-1749/secretsanta-generator.git'
            }
        }
        
    stage('Code-Compile') {
        steps {
                sh "mvn clean compile"
            }
        }
        
    stage('Unit Tests') {
        steps {
                sh "mvn test"
            }
        }
        
    
        
    stage('Sonarqube Analysis') {
        steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=santa \
                    -Dsonar.projectKey=santa -Dsonar.java.binaries=. '''
                }
            }
        }
    stage('OWASP Dependency Check') {
        steps {
            dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
              dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
    stage('Code-Build') {
        steps {
                sh "mvn clean package"
            }
        }
        
    stage('Docker Build') {
        steps {
            script{
              withDockerRegistry(credentialsId: 'docker') {
                  sh "docker build -t santa123 . "      
                  }
              }
            }
        }
        
    stage('Docker Tag & Push Image') {
        steps {
            script{
              withDockerRegistry(credentialsId: 'docker') {
                  sh "docker tag santa123 hello8989/santa123:latest"
                  sh "docker push hello8989/santa123:latest"
                  }
              }
            }
        }
        
    stage('Docker Image Scan') {
        steps {
               sh "trivy image hello8989/santa123:latest"
            }
        }
        
        

    }
}

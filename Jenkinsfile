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
        
    stage('OWASP Dependency Check') {
        steps {
            dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
              dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

    }
}

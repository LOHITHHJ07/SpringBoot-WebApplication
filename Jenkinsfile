pipeline {
    agent any
    
    tools {
        jdk 'openJDK11'
        maven 'maven'
    }
    
    environment{
        SCANNER_HOME= tool 'sonarScanner'
    }

    stages {
        stage('Git Checkout ') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/LOHITHHJ07/SpringBoot-WebApplication.git'
            }
        }
        
        stage('Code Compile') {
            steps {
                    sh "mvn compile"
            }
        }
        
        stage('Run Test Cases') {
            steps {
                    sh "mvn test"
            }
        }
        
        stage('Sonarqube Analysis') {
            steps {
                    withSonarQubeEnv('sonar_server') {
                        sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Java-WebApp \
                        -Dsonar.java.binaries=. \
                        -Dsonar.projectKey=Java-WebApp '''
    
                }
            }
        }
        
        // stage('OWASP Dependency Check') {
        //     steps {
        //            dependencyCheck additionalArguments: '--scan ./   ', odcInstallation: 'DP'
        //            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        
        stage('Maven Build') {
            steps {
                    sh "mvn clean install"
            }
        }
        
         stage('Build and Push Docker Image') {
            steps {
                  script {
            withDockerRegistry([credentialsId: '07eb5b42-8bb5-4721-81a6-8b9e546fbe97', url: 'https://index.docker.io/v1/']) {
                // Build the Docker image and tag it as 'webapp'
                sh "docker build -t webapp ."

                // Tag the built image correctly
                sh "docker tag webapp lohithhj/lohith_public_repo:latest"

                // Push the Docker image to Docker Hub
                sh "docker push lohithhj/lohith_public_repo:latest"
                   }
                }
            }
        }
        
        
        // stage('Docker Image scan') {
        //     steps {
        //             sh "trivy image adijaiswal/webapp:latest "
        //     }
        // }
        
    }
}

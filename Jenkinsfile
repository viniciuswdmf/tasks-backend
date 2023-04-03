pipeline {
    agent any
    stages{
        stage ('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests'){
            steps{
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR-LOCAL'){
                bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=ddc35680a13a1cabbbb80ef5e952496deecf126f -Dsonar.java.binaries=target"
                }
            }
        }
        stage ('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Test'){
            steps{
                dir('api-test'){
                    git 'https://github.com/viniciuswdmf/task-api-test'
                    bat 'mvn test'
                }
            }
        }
        stage ('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git 'https://github.com/viniciuswdmf/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage ('Functional Tests'){
            steps{
                dir('functional-test'){
                    git 'https://github.com/viniciuswdmf/tasks-functional-tests'
                    bat 'mvn test'
                }
            }
        }
    }
}



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
        stage ('Quality Gate'){
            steps{
                sleep(60)
                timeout(time:1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}



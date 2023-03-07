pipeline{
    agent{label 'AGENT'}
    triggers { pollSCM('* * * * *') }
    stages{
        stage('VCS'){
            steps{
                git url: 'https://github.com/abhish0916dev/spring-petclinic.git',
                    branch: 'declarative'
            }
        }
        stage('BUILD'){
            steps{
                sh 'mvn package'
            }
        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic3 -Dsonar.projectKey=springpetclinic3'
                }
            }
        }
        stage('Archieve Artifact'){
            steps{
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                        onlyIfSuccessful: true
            }
        }
        stage('Test Results'){
            steps{
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}
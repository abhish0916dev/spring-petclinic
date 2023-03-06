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
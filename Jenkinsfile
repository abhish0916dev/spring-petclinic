pipeline{
    agent{label 'AGENT'}
    trigeers{pollSCM('* * * * *')}
    stages{
        stage('Version Control System'){
            steps{
                git url: 'https://github.com/abhish0916dev/spring-petclinic.git',
                branch: 'release'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn package'
            }
        }
        stage('Post Build'){
            steps{
                archiveArtifacts artifacts: '**//target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true,
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}
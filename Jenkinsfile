pipeline{
    agent{label 'AGENT'}
    triggers{pollSCM('* * * * *')}
    stages{
        stage('Version Control System'){
            steps{
                git url: 'https://github.com/abhish0916dev/spring-petclinic.git',
                branch: 'release'
            }
        }
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv('Sonar_Cloud'){
                    sh ' mvn sonar:sonar -D sonar.organization=springpetclinic16 -Dsonar.projectKey=springpetclinic16'
                } // You can override the credential to be used
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
                onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
    post{
        success{
            mail subject: "Jenkins Job of ${JOB_NAME} of build no ${BUILD_ID} is successfull",
                 body: "For more info click on the URL - ${BUILD_URL}",
                 from: 'Jenkins@abhish.com',
                 to: 'abhish9416@outlook.com'
        }
        failure{
            mail subject: "Jenkins Job of ${JOB_NAME} of build no ${BUILD_ID} is fialed",
                 body: "For more info click on the URL - ${BUILD_URL}",
                 from: 'Jenkins@abhish.com',
                 to: 'abhish9416@outlook.com'
        }
    }
}
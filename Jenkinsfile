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
    }
}
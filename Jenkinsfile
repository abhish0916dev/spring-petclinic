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
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: 'https://abhish9416.jfrog.io/',
                    credentialsId: 'JFORG_CLOUD_PASSWD'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }
        stage('BUILD'){
            steps{
                rtMavenRun (
                    tool: 'MAVEN_3.6.3', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    // resolverId: "MAVEN_RESOLVER"
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
                // sh 'mvn package'
            }
        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SonarCloud') {
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
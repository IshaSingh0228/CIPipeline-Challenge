pipeline{
   agent any
   tools{
       maven 'maven3'
       jdk 'jdk11'
   }
  stages {
      stage("Maven Build"){
          steps{
              bat 'mvn clean compile'
          }
      }
      stage('Maven Test'){
            steps{
                bat 'mvn clean test'
            }
            post{
            always{
                junit 'target/surefire-reports/*.xml'
            }
        }
        }
     stage("Build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('sonar-server3') {
                bat 'java -version'
                bat 'mvn verify sonar:sonar'
              }
            }
          }
     
      stage('package'){
          steps{
          bat 'mvn package'
          }
}    
     
     stage("Uploading to artifactory"){
    steps{
    rtUpload (
    serverId: 'ARTIFACTORY_SERVER',
    spec: '''{
          "files": [
            {
              "pattern": "target/*.jar",
              "target": "art-doc-dev-loc-challenge"
            }
         ]
    }''',
)
}}
     
     
    }
  }

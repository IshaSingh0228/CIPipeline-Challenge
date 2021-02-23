pipeline{
   agent any
  stages {
      stage("Maven Build"){
          steps{
              sh 'mvn clean compile'
          }
      }
      stage('Maven Test'){
            steps{
                sh 'mvn clean test'
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
                sh 'java -version'
                sh 'mvn clean verify sonar:sonar'
              }
            }
          }
     stage('Deploy to artifactory'){
        steps{
        rtUpload(
         serverId : 'ARTIFACTORY_SERVER',
         spec :'''{
           "files" :[
           {
           "pattern":"target/*.jar",
           "target":"art-doc-dev-loc-challenge/
"
           }
           ]
         }''',
         
      )
      }
     }
    }
  }

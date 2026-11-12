pipeline {
  agent any
  tools { jdk 'JDK17'; maven 'Maven3' }

  stages {
    stage('Build & Deploy to Dev') {
      steps {
        checkout scm
        withCredentials([
          string(credentialsId: 'anypoint-client-id',     variable: 'CID'),
          string(credentialsId: 'anypoint-client-secret', variable: 'CSEC'),
          string(credentialsId: 'mule-secure-key',        variable: 'SKEY')
        ]) {
          configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            bat '''set "EXCHANGE_PASS=%CID%~?~%CSEC%"
mvn clean deploy -DmuleDeploy -DskipTests -s "%MAVEN_SETTINGS%" ^
  -Danypoint.client.id=%CID% -Danypoint.client.secret=%CSEC% ^
  -DsecureKey=%SKEY% -Danypoint.env=dev ^
  -Danypoint.bg="Aintiram Web Tech AWT" ^
  -Dapp.name=sample-learning-sys-api-dev'''
          }
        }
      }
    }
  }
}
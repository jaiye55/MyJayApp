node {

def mvnHome = tool 'Maven3'
stage ('Checkout') {

checkout scm
}
stage ('Build') {
sh "${mvnHome}/bin/mvn clean install -f MyJayApp/pom.xml"
}
stage ('Code Quality Scan') {
   def scannerHome = tool 'SonarScanner 4.0'; {
withSonarQubeEnv('SonarQube') {
sh "${mvnHome}/bin/mvn sonar:sonar -f MyJayApp/pom.xml"
   }
}
stage ('DEV Deploy')  {
      echo "deploying to DEV Env "
      deploy adapters: [tomcat8(credentialsId: 'da622b00-f7f3-4a3a-b4fc-985bdfe6000b', path: '', url: 'http://44.203.161.68:8080/')], contextPath: null, war: '**/*.war'
    }
stage ('DEV Approve') {
echo "Taking approval from DEV"
timeout(time: 7, unit: 'DAYS') {
input message: 'Do you want to deploy?', submitter: 'admin'
     }
 }
stage ('Slack Notification') {


    slackSend(channel:'myjayeapp', message: "Job is successful, here is the info -  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

  }

}

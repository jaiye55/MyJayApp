node {

def mvnHome = tool 'Maven3'
stage ('Checkout') {

checkout scm
}
stage ('Build') {
sh "${mvnHome}/bin/mvn clean install -f MyJayApp/pom.xml"
}
stage ('Code Quality Scan') {
withSonarQubeEnv('SonarQube') {
sh "${mvnHome}/bin/mvn sonar:sonar -f MyJayApp/pom.xml"
   }
}
stage ('DEV Deploy')  {
      echo "deploying to DEV Env "
      deploy adapters: [tomcat9(credentialsId: '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', path: '', url: 'http://54.205.157.155:8080/')], contextPath: null, war: '**/*.war'
    }
stage ('DEV Approve') {
echo "Taking approval from DEV"
timeout(time: 7, unit: 'DAYS') {
input message: 'Do you want to deploy?', submitter: 'admin'
     }
 }
stage ('Slack Notification') {


    slackSend(channel:'myjayapp', message: "Job is successful, here is the info -  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

  }

}

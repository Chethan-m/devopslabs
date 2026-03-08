
  node {
 
 def mavenHome = tool name: 'maven3.9.12'
 
 try {
 
stage('checkoutcode') {
sendSlackNotifications('STARTED')
git branch: 'development', url: 'https://github.com/Chethan-m/devopslabs.git'

 } 
stage('Build') {

sh "${mavenHome}/bin/mvn clean package"

}
stage('ExecuteSonarQubeReport') {

sh "${mavenHome}/bin/mvn clean sonar:sonar"

}
stage('UploadArtifactIntoNexus') {

sh "${mavenHome}/bin/mvn clean deploy"

}
stage('DeployAppintoTomcat') {
sshagent(['9e76a4c6-1d96-4aa5-9347-856f214a05b7']) {
sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/scripted-pipeline/target/maven-web-application.war ubuntu@172.31.3.183:/opt/apache-tomcat-9.0.115/webapps"

	}
  }
 
 }
 catch (e) {
 currentBuild.result = "FAILED"
 throw e
 }
 
 finally {
 
 sendSlackNotifications(currentBuild.result)
 }
 
 }//node finish
 
 def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def color = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#walmart-developers')
}
    
   
       
   

  

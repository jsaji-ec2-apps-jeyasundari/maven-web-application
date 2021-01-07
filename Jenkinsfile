node ('development')
{

def mavenHome = tool name: "maven3.6.3"
  
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
  
  // properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '2')), pipelineTriggers([pollSCM('* * * * *')])])

  stage ('CheckoutCode')
  {
  git branch: 'development', credentialsId: '6c882d31-cf86-4bad-9e57-a166971a1653', url: 'https://github.com/jsaji-ec2-apps-jeyasundari/maven-web-application.git'
  }

  stage ('build')
  {
  sh "${mavenHome}/bin/mvn clean package"
  }

  stage ('ExecuteSonarQubeReport')
  {
  sh "${mavenHome}/bin/mvn sonar:sonar"
  }

  stage ('uploadArtifactInfoNexus')
  {
  sh "${mavenHome}/bin/mvn deploy"
  }

  stage ('DeployAppIntoTomcatserver')
  {
  sshagent(['a9a479d9-c846-4016-932c-c695e9555002'])
  {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.230.43:/opt/apache-tomcat-9.0.41/webapps/"
  }
  }

  stage ('SendEmailNotification')
  {
  mail bcc: 'akboominathan.com', body: '''Build over guyss

  Regards,
  jeya
  8825623075.''', cc: 'akboominathan.r@gmail.com', from: '', replyTo: '', subject: 'Build over!!!', to: 'kjeyaabikan595@gmail.com'
  }
}

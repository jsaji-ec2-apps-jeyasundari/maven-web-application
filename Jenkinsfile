node 
{

def mavenHome = tool name: "maven3.6.3"

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
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.120.224:/opt/apache-tomcat-9.0.41/webapps/"
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

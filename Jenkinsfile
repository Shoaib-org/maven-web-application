node
{
    def mavenHome = tool name: "maven 3.8.1"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'eb939470-22f5-4bfd-8a9e-6e4621824cf7', url: 'https://github.com/Shoaib-org/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
} 
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactsintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppintoTomcatServer')
{
    sshagent(['2a0bc723-4a1a-4c34-9de9-41ba829b76d7']){
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.37.66:/opt/apache-tomcat-9.0.46/webapps/"
    }
    
}
stage('SendEmailNotification')
{
emailext body: '''Build Over.


Regards
Mithun Technologies''', subject: 'Build Over', to: 'shoaib989@gmail.com'
}
}

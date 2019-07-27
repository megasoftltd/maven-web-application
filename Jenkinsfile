node()
{
      def mvnHome=tool name: 'Maven-3.6.1', type: 'maven'
      properties([
        buildDiscarder(logRotator(numToKeepStr: '5')),
        pipelineTriggers([
        pollSCM('* * * * *')])
      ])
   
    stage('Checkoutcode')
    {
        git branch: 'development', credentialsId: '02df2490-a555-4b80-a215-cfea02519a6b', url: 'https://github.com/megasoftltd/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage('execute sonar report')
    {
       sh "${mvnHome}/bin/mvn sonar:sonar"  
    }
    stage("upload artifactory")
    {
        sh "${mvnHome}/bin/mvn deploy"
    }
    stage('deploy into tomcat')
    {
       sshagent(['03276880-9cea-4d4e-99eb-fbbbc3da34b8']) 
       {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.72.242:/opt/apache/apache-tomcat-9.0.21/webapps/maven-web-application.war"
       } 
    }
    stage('Email Notification')
    {
        emailext body: '''Build is Over, please check the logs for more information.

    Regards
    Sivaprasad
    Ph:949 100 3444''', subject: 'Build Status', to: 'sivaprasad.boddupalli@gmail.com'
    }
}   

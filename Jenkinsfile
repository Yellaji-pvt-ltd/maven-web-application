node {
    def mavenHome = tool name: "maven3.8.4"
    //properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage ('CheckoutCode'){
        git branch: 'development', url: 'https://github.com/Yellaji-pvt-ltd/maven-web-application.git'
    }
    
    stage ('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage ('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage ('UploadArtifactIntoNexusRepo'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage ('DeployAppIntoTomcatServer'){
        sshagent(['1265b21d-1ea0-4b65-a0e8-b4284ae243c8']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.109.122.176:/opt/apache-tomcat-9.0.58/webapps/"
        }
    }
}

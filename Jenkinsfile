pipeline {
  agent any 
    tools {
       //Installed the maven version configured as 'M3' and add it to the path
     jdk 'Java-8'
     maven 'Maven'
  }
  environment {

      sonar_url = 'http://3.137.155.203/:9000'
      sonar_username = 'admin'
      sonar_password = 'admin'
      nexusUrl = 'http://3.137.155.203/:8081'
 }
  
  stages 
  {
   stage('Git Clone') {
     steps {
       // get some code from a GitHub repository
       git 'https://github.com/madhupriya1121/hellowold.git'
    }
  }
  stage('compile and build') {
      steps {
          sh '''
          mvn clean install -U -Dmaven.skip.test=true
          '''
      }
  }
  stage ('Sonarqube Analysis'){
           steps {
           withSonarQubeEnv('sonarqube') {
           sh '''
           mvn clean package org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=false
           mvn -e -B sonar:sonar -Dsonar.java.source=1.8 -Dsonar.host.url="${sonar_url}" -Dsonar.login="${sonar_username}" -Dsonar.password="${sonar_password}" -Dsonar.sourceEncoding=UTF-8
           '''
           }
         }
      }
   stage ('Publish Artifact') {
        steps {
          nexusArtifactUploader artifacts: [[artifactId: '${Service}', classifier: '', file: "target/${Service}-${env.artifact_version}.jar", type: 'jar']], credentialsId: 'ca1d8e48-50b5-46c5-ba0c-7762f4403926', groupId: 'com.sapient.mc', nexusUrl: "${nexus_url}", nexusVersion: 'nexus3', protocol: 'http', repository: 'releases', version: "${artifact_version}"
        }
      }
}
}

pipeline {
  agent any 
    tools {
       //Installed the maven version configured as 'M3' and add it to the path
     jdk 'Java-8'
     maven 'Maven'
  }
  environment {

      sonar_url = 'http://172.31.44.88:9000'
      sonar_username = 'admin'
      sonar_password = 'admin'
      nexus_url = '172.31.44.88:8081'
      artifact_version = '4.0.0'
      
      
      
 }
  
  stages 
  {
   stage('Git Clone') {
     steps {
       // get some code from a GitHub repository
        git branch: 'main',
        url: 'https://github.com/madhupriya1121/hellowold.git'    
         
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
           mvn -e -B sonar:sonar -Dsonar.java.source=1.8 -Dsonar.host.url="${sonar_url}" -Dsonar.login="${sonar_username}" -Dsonar.password="${sonar_password}" -Dsonar.sourceEncoding=UTF-8
           '''
           }
         }
      } 
stage ('Publish Artifact') {
        steps {
          nexusArtifactUploader artifacts: [[artifactId: 'hello-world-war', classifier: '', file: "target/hello-world-war-1.0.0.war", type: 'war']], credentialsId: 'a6deda24-cd83-4719-88af-ea3c1812621d', groupId: 'com.efsavage', nexusUrl: "${nexus_url}", nexusVersion: 'nexus3', protocol: 'http', repository: 'release', version: "${artifact_version}"
        }
      }
}
}

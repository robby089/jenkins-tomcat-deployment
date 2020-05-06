pipeline {
   
 agent any
stages {
   
stage ('check_out')
{
steps {
    git credentialsId: 'Robby089', url: 'https://github.com/robby089/jenkins-tomcat-deployment.git'
}    
}
 stage ('java_compile')
{
steps {
    tool name: 'JDK18', type: 'jdk'
    tool name: 'local', type: 'maven'
   
 bat'mvn -Dintegration-tests.skip=true clean compile'  
}    
}  
   
stage ('mvn_test')
{
steps {
    tool name: 'JDK18', type: 'jdk'
    tool name: 'local', type: 'maven'  
    bat'mvn -Dintegration-tests.skip=true  test package'
   
}    
}

stage ('email_DeploymentAppoval')
{
steps {
       
     emailext body: '$DEFAULT_CONTENT', subject: 'Need Your Approval', to: 'robertdevops9@gmail.com'
   
}    
}

stage ('sonar_scan')
{
      input {
        message "Should we continue?"
   
      }
steps {
      tool name: 'JDK18', type: 'jdk'
      tool name: 'local', type: 'maven'
      tool name: 'sonartest', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      bat 'mvn clean verify sonar:sonar'
   
}    
}


stage ('Deploying_File')
  {
steps {
   deploy adapters: [tomcat9(credentialsId: 'Tomcat9', path: '', url: 'http://localhost:8089')], contextPath: 'jenkins-tomcat', war: 'target\\jenkins-tomcat.war'
}    
}

}
}

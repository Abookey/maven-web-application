pipeline {
  agent {
    label 'Agent3'
  }
  
  environment {
    MAVEN_HOME = tool name: 'maven2'
  }
  
  stages{
    stage('1.GetCode') {
      steps {
        git "https://github.com/Abookey/maven-web-application"
        //sh "git clone https://github.com/Abookey/maven-web-application"
        //bat "git clone https://github.com/Abookey/maven-web-application"
      }
    }
    
    stage('2.Build') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn package"
      }
    }
    
    stage('3.codeQualityAnalysis') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn sonar:sonar"
      }
    }
    
    stage('4.upload') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn deploy"
      }
    }
    
    stage('5.deploy2UAT') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://34.236.155.53:8187/')], contextPath: null, war: 'target/*war'
      }
    }
    
    stage('6.Approval') {
      steps {
        echo 'Apps ready for review'
        timeout(time: 5, unit: 'HOURS') {
          input message: 'Application ready for deployment. Please review and approve.'
        }
      }
    }
  }
}

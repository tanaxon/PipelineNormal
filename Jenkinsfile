pipeline {

  agent any

  stages {

    /* Build */
    stage('Build') {
      steps {
        dir ('Pipeline') {
      	  bat 'mvn clean package install -DskipTests=true -DskipWebServicesSourcesCreation=false'
          archiveArtifacts 'target/*.iar'
        }
      }
    }

    /* Deploy - Only for master branch */
    stage('Deploy') {
      when {
        branch 'master'
      }
      steps {
        script {
          def CURRENT_BUILD = currentBuild.number
          echo "Current build number : $CURRENT_BUILD"

            dir ('Pipeline') {
              httpRequest ignoreSslErrors: true, 
              responseHandle: 'NONE', 
              url: "http://10.123.0.174:8085/ivy/pro/IvyDeploymentTool/deployment_tool_ivy/144CBB1313C1F669/start.ivp?applicationName=PIPELINE&useActiveDirectory=false&redeploy=true&newApplication=false&downloadUrl=http%3A%2F%2F10.123.0.174%3A9000%2Fjob%2FPipelineNormal2F$CURRENT_BUILD%2Fartifact%2Ftarget%2Fpipeline-1.0.0-SNAPSHOT.iar", 
              validResponseCodes: '200'
            }
        }
      }
    }
  }

  /* Final */
  post {
      always {
          echo 'Finished'
          //deleteDir() /* clean up our workspace */
      }
      success {
          echo 'Succeeeded!'
      }
      unstable {
          echo 'Unstable :/'
      }
      failure {
          echo 'Failed :('
      }
      changed {
          echo 'Things were different before...'
      }
  }
  
}

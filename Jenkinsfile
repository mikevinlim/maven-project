pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success{
          echo "Now Archiving..."
          archiveArtifacts artifacts:'**/*.war'
        }
      }
    }
    stage('Deploy to staging') {
      steps {
        build job: 'DeployToStaging'
      }
      stage('Deploy to prod') {
        steps {
          timeout(time:5, unit:'DAYS'){
            input message: 'Approve PRODUCTION Deployment?'
          }
          build job: 'DeploytoProd'
        }
        post{
          success{
            echo ' Code deployed to prod'
          }
          failure{
            echo 'Deployment Failed'
          }
        }
      }
    }    
  }
}

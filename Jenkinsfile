pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo-app'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Linux tests') {
      parallel {
        stage('Linux tests') {
          steps {
            echo 'Run Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows Tests') {
          steps {
            echo 'Run Windows tests'
          }
        }

      }
    }

    stage('Deploy Staging') {
      steps {
        echo 'Deploy to staging environment'
        input 'Ok to deploy to production?'
      }
    }

    stage('Deploy Production') {
      post {
        always {
          archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
        }

      failure {
        mail to: 'joseht8@gmail.com',
        subject: "Failed Pipeline ${currentBuild.fullDisplayName}",
        body: " For details about the failure, see ${env.BUILD_URL}"
        }
      }
      steps {
        echo 'Deploy to Prod'
      }
    }
    
  }
}


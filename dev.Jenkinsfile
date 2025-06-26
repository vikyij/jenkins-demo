pipeline {
    agent any

    environment {
        APP_NAME = 'Jenkins-demo'
    }

   stages {
    stage('clone repo') {
        steps {
            echo "cloning code for $APP_NAME"
            git url: 'https://github.com/vikyij/jenkins-demo', branch: 'main'
        }
    }

    stage ('build project') {
        steps {
            echo "Building $APP_NAME"
        }
    }

    stage ('Custom Message') {
        steps {
            script {
                def timestamp = new Date().format("yyyy-MM-dd HH:mm:ss")
                echo "Pipeline completed at $timestamp"
            }
        }
    }
    stage ('Notify') {
        steps {
            echo "Pipeline Done"
        }
    }

   }

   post {
     always {
      echo '📦 Done with pipeline (success or fail)'
    }
   }
}
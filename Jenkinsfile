pipeline {
  agent none
  stages {
    stage('build') {
      steps {
        echo 'this is the build job'
        sh 'npm install'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'lagairogo/node'
        }

      }
      steps {
        echo 'this is the test job'
        sh '''npm install
npm test'''
      }
    }

    stage('package') {
      agent {
        docker {
          image 'lagairogo/node'
        }

      }
      steps {
        echo 'this is the package job'
        sh '''npm install
npm run package'''
      }
    }

    stage('Build and Publish') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("Angshuman-dev/shopping-portal:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
          }
        }

      }
    }

  }
  tools {
    nodejs 'nodejs'
  }
  post {
    always {
      echo 'this pipeline has completed...'
    }

  }
}
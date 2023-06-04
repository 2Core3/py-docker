pipeline {
  agent {
         label 'vagrant'
         }
  stages {
    stage('Build') {
      steps {
        script {
        sh 'docker build -t my-image .'
        }
      }
    }
    stage('Run') {
      steps {
          sh 'docker run -d -p 80:80 --name my-image my-image gunicorn --bind 0.0.0.0:80 src.core.wsgi:app'
      }
    }

    stage('Test') {
      steps {
        sh 'ip ad'
        sh 'curl 192.168.56.3:80'
      }
    }
    stage('Push') {
      steps {
        script {
          docker.withRegistry('http://192.168.32.3:5000') {
            docker.image('my-image').push()
          }
        }
      }
    }
    stage('clean') {
      steps {
          sh 'docker stop my-image'
          sh 'docker rm my-image'
      }
    }

  }
  post {
      failure {
        sh 'docker stop my-image'
        sh 'docker rm my-image'

      }
      always {
        sh 'docker image rm my-image'  
      }
  }
}

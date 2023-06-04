pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          docker.build('my-image')
        }
      }
    }
    stage('Run') {
      steps {
          sh 'docker run -d -p 8000:8000 --name my-image my-image gunicorn --bind 0.0.0.0:8000 src.core.wsgi:app'
      }
    }

    stage('Test') {
      steps {
        sh 'ip ad'
        sh 'curl 192.168.56.113:8000'
      }
    }
    stage('Push') {
      steps {
        script {
          docker.withRegistry('http://172.21.0.2:5000') {
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


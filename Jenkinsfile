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
                // Шаги сборки образа и публикации
                script {
                    docker.image('my-image:latest').tag('192.168.96.2:5000/my').push()
                    docker.image('192.168.96.2:5000/my').push()
                }

                // Шаг проверки реестра
                sh 'curl -X GET http://192.168.96.2:5000/v2/_catalog'
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

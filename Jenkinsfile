pipeline {
  agent any

  stages {

    stage('Build') {
      steps {
        sh 'docker build -t $JOB_NAME .'
      }
    }

    stage('Deploy') {
        steps {
          sh """
            docker stop $JOB_NAME || true
            docker rm $JOB_NAME || true

            docker run -d \
              --name $JOB_NAME \
              --network apps \
              -l traefik.enable=true \
              -l 'traefik.http.routers.${JOB_NAME}.rule=Host(`${JOB_NAME}.feedback.com.mx`)' \
              -l 'traefik.http.services.${JOB_NAME}.loadbalancer.server.port=3001' \
              $JOB_NAME
          """
        }
      }

    stage('Debug') {
      steps {
        sh 'echo JOB_NAME=$JOB_NAME'
        sh 'echo BUILD_NUMBER=$BUILD_NUMBER'
      }
    }
  }
}



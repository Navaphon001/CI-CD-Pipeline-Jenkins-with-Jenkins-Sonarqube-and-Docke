pipeline {
  agent any

  environment {
    SONAR_HOST_URL = 'http://host.docker.internal:9001'  // ถ้า SonarQube รันบนเครื่องโฮสต์ Windows
  }

  stages {
    stage('Maven Check') {
      steps {
        sh '''
          docker run --rm \
            maven:3.9-eclipse-temurin-17 \
            mvn -v
        '''
      }
    }

    stage('Build') {
      steps {
        sh '''
          docker run --rm \
            --add-host=host.docker.internal:host-gateway \
            -v "$WORKSPACE:/usr/src/mymaven" \
            -w /usr/src/mymaven \
            maven:3.9-eclipse-temurin-17 \
            mvn -B clean install
        '''
      }
    }

    stage('SonarQube') {
      steps {
        withCredentials([string(credentialsId: 'sonar-token-id', variable: 'SONAR_TOKEN')]) {
          sh '''
            docker run --rm \
              --add-host=host.docker.internal:host-gateway \
              -v "$WORKSPACE:/usr/src/mymaven" \
              -w /usr/src/mymaven \
              maven:3.9-eclipse-temurin-17 \
              mvn -B clean verify sonar:sonar \
                -Dsonar.projectKey=TestCI \
                -Dsonar.projectName=TestCI \
                -Dsonar.host.url="$SONAR_HOST_URL" \
                -Dsonar.token="$SONAR_TOKEN"
          '''
        }
      }
    }
  }

  post {
    always { echo 'Pipeline completed.' }
  }
}

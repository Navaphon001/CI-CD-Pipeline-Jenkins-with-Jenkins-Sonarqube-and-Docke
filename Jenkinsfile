pipeline {
    agent any

    stages {
        stage('Maven Check') {
            steps {
                sh 'docker run -i --rm --name my-maven-project maven:3.9.9 mvn --version'
            }
        }
        stage('Build') {
            steps {
                sh 'docker run -i --rm --name my-maven-project -v "C:\Users\Admin\Documents\PSU\Year4\Term1\Mobile\Kitwara\Learn Jenkins\Test\CI CD\java-hello-world-with-maven\\myapp:/usr/src/mymaven" -w /usr/src/mymaven maven:3.9.9 mvn clean install'
            }
        }
        stage('SonarQube') {
            steps {
                sh '''#!/bin/sh
                docker run -i --rm --name my-maven-project \
                  -v "C:\\Users\\Admin\\Documents\\PSU\\Year4\\Term1\\Mobile\\Kitwara\\Learn Jenkins\\Test\\CI CD\\java-hello-world-with-maven:/usr/src/mymaven" \
                  -w /usr/src/mymaven maven:3.9.9 \
                  mvn clean verify sonar:sonar \
                  -Dsonar.projectKey=TestCI \
                  -Dsonar.projectName="TestCI" \
                  -Dsonar.host.url=http://host.docker.internal:9001 \
                  -Dsonar.token=sqp_095f871d780127e18b2d112cc9a9bd95e7d7698a
                '''
            }
        }
    }
}

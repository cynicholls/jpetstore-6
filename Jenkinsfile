pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build Started'
        sh '''wget -c https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.tar.gz -O - | tar -xz -C /tmp
export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn clean install -Dlicense.skip=true'''
        echo 'Build Finished'
      }
    }

    stage('Testing') {
      steps {
        sh 'mvn sonar:sonar -Dsonar.host.url=https://8403d8d4-e355-4e00-9ed1-4d2e8bacc5d7-sonarqube9000.sessions.dev.skillslounge.io -Dlicense.skip=true'
      }
    }

  }
}
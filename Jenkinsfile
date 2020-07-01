pipeline {
  agent any
  stages {
    stage('Prepare Maven') {
      steps {
        echo 'Downloading Maven'
        sh 'wget -c https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.tar.gz -O - | tar -xz -C /tmp'
        echo 'Download Maven Complete'
      }
    }

    stage('Build') {
      steps {
        echo 'Build Started'
        sh '''export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn clean package -Dlicense.skip=true'''
        echo 'Build Complete'
      }
    }

    stage('Testing') {
      steps {
        sh ' mvn sonar:sonar -Dsonar.host.url=https://58ea782a-85f3-41eb-af25-56e8d1f1052d-sonarqube9000.sessions.dev.skillslounge.io -Dlicense.skip=true'
      }
    }

  }
}
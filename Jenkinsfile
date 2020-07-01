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
        sh '''export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn sonar:sonar -Dsonar.host.url=https://58ea782a-85f3-41eb-af25-56e8d1f1052d-sonarqube9000.sessions.dev.skillslounge.io -Dlicense.skip=true'''
      }
    }

    stage('JFrog Push') {
      steps {
        script {
          def server = Artifactory.server "artifactory"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean package -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

      }
    }

  }
  tools {
    maven 'maven'
  }
}
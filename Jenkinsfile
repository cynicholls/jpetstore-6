pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Start Build'
        sh '''wget -c https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.tar.gz -O - | tar -xz -C /tmp
export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn clean package -Dlicense.skip=true'''
        echo 'Build Complete'
      }
    }

    stage('Testing') {
      parallel {
        stage('SonarQube Tests') {
          steps {
            sh '''export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn sonar:sonar -Dsonar.host.url=https://3b4e5a92-2f71-4363-8083-8b2cd7d9ec51-SonarQube9000.sessions.dev.skillslounge.io -Dlicense.skip=true'''
          }
        }

        stage('Print Tester Credentials') {
          steps {
            echo "The tester is ${TESTER}"
            sleep 10
          }
        }

        stage('Print Build Number') {
          steps {
            echo "This is build number ${BUILD_ID}"
            sleep 20
          }
        }

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

    stage('Deploy Prompt') {
      steps {
        input 'Deploy to Production?'
      }
    }

    stage('Deployment') {
      steps {
        echo 'Dummy Prod Deploy'
      }
    }

  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'John Smith'
  }
}
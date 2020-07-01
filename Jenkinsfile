pipeline {
  agent any
  stages {
    stage('Pre-reqs') {
      parallel {
        stage('Prepare Maven') {
          steps {
            echo 'Downloading Maven'
            sh 'wget -c https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.tar.gz -O - | tar -xz -C /tmp'
            echo 'Download Maven Complete'
          }
        }

        stage('Prepare Chromedriver') {
          steps {
            echo 'Download Chromedriver'
            sh '''CHROME_HOME="/var/jenkins_home/.m2/repository/webdriver/chromedriver/linux64/83.0.4103.39"
wget -c https://chromedriver.storage.googleapis.com/83.0.4103.39/chromedriver_linux64.zip -O chromedriver.zip
unzip chromedriver.zip
mkdir -p $CHROME_HOME
mv chromedriver $CHROME_HOME
ls -l $CHROME_HOME'''
          }
        }

      }
    }

    stage('Build') {
      steps {
        echo 'Build Started'
        sh '''export PATH=$PATH:/tmp/apache-maven-3.6.3/bin
mvn clean install -Dlicense.skip=true'''
        echo 'Build Complete'
      }
    }

  }
}
pipeline {
  agent any
  stages {
    stage('SCM Clone Repository') {
      steps {
        git(url: 'https://github.com/ravikirankiran097/Spring-PetClinic.git', branch: 'main', credentialsId: 'GitHub_Cred')
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean -DskipTests=true install'
      }
    }

    stage('Unit Test') {
      parallel {
        stage('Unit Test') {
          steps {
            sh 'mvn -Dintegration-tests.skip=true -Dmaven.test.failure.ignore=true -Dcheckstyle.skip test'
          }
        }

        stage('Integration Test') {
          steps {
            sh 'mvn -Dunit-tests.skip=true -Dcheckstyle.skip=true verify'
          }
        }

      }
    }

    stage('Junit Result Archieve') {
      steps {
        junit(allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml')
      }
    }

  }
  tools {
    maven 'M3'
  }
}
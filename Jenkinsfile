#!/usr/bin/env groovy

pipeline {
  agent {
    node {
      label 'jenkins-lts-1-rtklg'
    }

  }
  stages {
    stage('checkout') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        sh 'checkout scm'
      }
    }
    stage('check java') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        sh '''sh "java -version"
'''
      }
    }
    stage('clean') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        sh '''sh "chmod +x mvnw"
sh "./mvnw clean"'''
      }
    }
    stage('install tools') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        sh '''sh "./mvnw com.github.eirslett:frontend-maven-plugin:install-node-and-yarn -DnodeVersion=v8.9.4 -DyarnVersion=v1.3.2"
'''
      }
    }
    stage('yarn install') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        catchError() {
          sh '''sh "./mvnw test"
'''
          sh '''junit \'**/target/surefire-reports/TEST-*.xml\'
'''
        }

      }
    }
    stage('frontend tests') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        catchError() {
          sh '''sh "./mvnw com.github.eirslett:frontend-maven-plugin:yarn -Dfrontend.yarn.arguments=test"
'''
          sh '''junit \'**/target/test-results/karma/TESTS-*.xml\'
'''
        }

      }
    }
    stage('packaging') {
      agent {
        node {
          label 'jenkins-lts-1-rtklg'
        }

      }
      steps {
        sh '''sh "./mvnw verify -Pprod -DskipTests"
archiveArtifacts artifacts: \'**/target/*.war\', fingerprint: true'''
      }
    }
  }
}

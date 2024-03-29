pipeline {
  agent {
    label 'maven'
  }
  environment {
    PROJECT = 'jenkins'
  }   
  stages {
    stage('Preamble') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject("${env.PROJECT}") {
              echo "Using project: ${openshift.project()}"
            }
          }
        }
      }
    }
    stage('Build') {
      steps {
        echo 'Build jar file'
        script {
          maven {
            goals('clean')
            goals('install')
            properties skipTests: true
          }
        }
      }
    }
    stage('Run Unit Tests') {
      steps {
        echo 'Run unit tests'
        script {
          maven {
            goals('test')
          }
        }
      }
    }
    stage('Delete') {
      steps {
        echo "Delete existing application"
        script {
          openshift.withCluster() {
            openshift.withProject("${env.PROJECT}") {
              openshift.selector("all", [  'app' : 'nginx-example' ]).delete()
            }
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        echo "Deploy application"
        script {
          openshift.withCluster() {
            openshift.withProject("${env.PROJECT}") {
              openshift.newApp('jenkins/nginx-example', '-p SOURCE_REPOSITORY_URL=https://github.com/apoorvajagtap/jenkins-ocp/', '-p NAME=jenkins-cicd-example')
            }
          }
        }
      }
    }
  }
}
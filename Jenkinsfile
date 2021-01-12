#!env groovy

String TF_REPO="https://github.com/mayadata-io/mayastor-terraform-playground"
String TF_BRANCH="main"
String TF_SUBDIR="mayastor-terraform-playground"

pipeline {
  stages {
    agent { label 'nixos-mayastor' }
    parallel {
      stage('init') {
        steps {
          step(
            sh 'sleep 5'
          )
        }
      }
      stage('terraform_checkout') {
        steps {
          step([
            $class: 'GitSCM',
            url: TF_REPO,
            branches: [[name: '*/${TF_BRANCH}']],
            extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '${TF_SUBDIR}']],
          ])
        }
      }
    }
    parallel {
      stage('linter') {
        step(
          sh 'sleep 5'
        )
      }
      stage('create_cluster') {
        step(
          sh 'cd ${TF_SUBDIR} && terraform init'
          sh 'cd ${+TF_SUBDIR} && terraform validate'
        )
      }
    }
  }
}

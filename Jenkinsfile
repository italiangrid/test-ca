#!/usr/bin/env groovy

pipeline {

  agent any
  
  options {
	  buildDiscarder(logRotator(numToKeepStr: '5'))
	  timeout(time: 1, unit: 'HOURS')
  }

  triggers { cron('@daily') }

  environment {
    WORKDIR = "/tmp/${env.BUILD_TAG}/artifacts"
  }

  stages {
    stage('build') {
      steps {
        sh 'make clean'
        sh 'make'
      }
    }
    stage('archive') {
      steps {
        sh "mkdir -p ${env.WORKDIR}"
        dir('igi-test-ca') {
          sh "cp *.tar.gz ${env.WORKDIR}"
          sh "cp rpmbuild/RPMS/noarch/*.rpm ${env.WORKDIR}"
        }
        dir("${env.WORKDIR}") {
          sh "ls -l ."
          archiveArtifacts '**'
        }
      }
    }
  }
}

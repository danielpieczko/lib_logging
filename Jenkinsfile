@Library('xmos_jenkins_shared_library@v0.15.0') _

getApproval()

pipeline {
  agent {
    label none
  }
  stages {
    stage('Standard build and XS2 tests') {
      agent {
        label 'x86_64&&brew'
      }
      environment {
        REPO = 'lib_logging'
        VIEW = getViewName(REPO)
      }
      options {
        skipDefaultCheckout()
      }
      stages{
        stage('Get view') {
          steps {
            xcorePrepareSandbox("${VIEW}", "${REPO}")
          }
        }
        stage('Library checks') {
          steps {
            xcoreLibraryChecks("${REPO}")
          }
        }
        stage('Tests') {
          steps {
            runXmostest("${REPO}", 'tests')
          }
        }
        stage('xCORE builds') {
          steps {
            dir("${REPO}") {
              xcoreAllAppsBuild('examples')
              xcoreAllAppNotesBuild('examples')
              dir("${REPO}") {
                runXdoc('doc')
              }
            }
          }
        }
      }// stages
      cleanup {
        xcoreCleanSandbox()
      }
    }//Stage standard build
    stage('XS3 Verification'){
      agent {
        label 'xcore.ai-explorer'
      }
      println 'Dummy stage on XS3'
    }
  }
  post {
    success {
      updateViewfiles()
    }
  }
}

pipeline {
  agent any
  environment {
    APPSYSID = '94cbcdea1b34e110e32d877ddc4bcb8e'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = 'ServiceNow'
    DEVENV = 'https://dev89399.service-now.com/'
    TESTENV = 'https://dev89399.service-now.com/'
    PRODENV = 'https://dev89399.service-now.com/'
    TESTSUITEID = '94cbcdea1b34e110e32d877ddc4bcb8e'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}

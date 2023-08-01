@Library('semgrep') _

pipeline {
  agent any
    environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      SEMGREP_BASELINE_REF = "origin/master"
      SEMGREP_BRANCH = "${BRANCH_NAME}"
      SEMGREP_COMMIT = "${GIT_COMMIT}"
      SEMGREP_REPO_URL = "https://github.com"
      SEMGREP_PR_ID = "${env.CHANGE_ID}"
      SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*)$/, '$1')
    }
    stages {
      stage('Print-Vars') {
        steps {
          sh 'printenv | sort'
        }
      }
      stage('Semgrep-Scan') {
        steps {
                script {
                    if (env.GIT_BRANCH == 'master') {
                        echo "we're checking out ${SEMGREP_REPO_NAME} on ${SEMGREP_BRANCH} ${SEMGREP_BASELINE_REF} with ${SEMGREP_REPO_URL}"
                        checkoutRepo(env.SEMGREP_REPO_NAME, env.SEMGREP_BRANCH, 100, env.SEMGREP_BASELINE_REF, env.SEMGREP_REPO_URL)
                        echo "Hello from ${env.GIT_BRANCH} branch"
                        semgrepFullScan()
                    }  else {
                        sh "echo 'Hello from ${env.GIT_BRANCH} branch'"
                        echo "we're checking out ${SEMGREP_REPO_NAME} on ${SEMGREP_BRANCH} ${SEMGREP_BASELINE_REF} with ${SEMGREP_REPO_URL}"
                        checkoutRepo(env.SEMGREP_REPO_NAME, env.SEMGREP_BRANCH, 100, env.SEMGREP_BASELINE_REF, env.SEMGREP_REPO_URL)
                      //sh "git fetch origin +ref/heads/*:refs/remotes/origin/*" //Is it needed?
                        semgrepPullRequestScan()
                    }
                }
            }
        }
    }
}

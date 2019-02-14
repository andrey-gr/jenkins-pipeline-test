pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'FIrst stage'
      }
    }
    stage('Proceed to stage?') {
      steps {
        input 'Should we deploy to stage?'
      }
    }
      stage('Tag & Release') {
        steps {
            script {
              def dateStr = new Date().format('yyyy-MM-dd')
              versionTag = "autohub-${dateStr}-#${env.BUILD_ID}"
            }

          sh 'git config user.email "jenkins@jenkins.fino.tech"'
          sh 'git config user.name "Jenkins Pipeline"'
          sh "git tag -a $versionTag -m 'Version deploy'"
          withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GITHUB_TOKEN', usernameVariable: 'GITHUB_USERNAME')]) {
              echo "$env.GITHUB_TOKEN"
              echo "$env.GITHUB_USERNAME"
              sh "git push https://${GITHUB_USERNAME}:${GITHUB_TOKEN}@github.com/andrey-gr/jenkins-pipeline-test.git --tags"
              sh "github-release release -u andrey-gr -r jenkins-pipeline-test --tag '${versionTag}' --name '$versionTag' --description \"\nBuild log:\n${env.BUILD_URL}\" --draft --pre-release"
          }
        }
      }
  }
}